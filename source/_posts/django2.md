---
title: django 2.0（二）使用uWSGI和Nginx部署线上环境
date: 2018-01-10 18:36:52
tags:
- uWSGI
- Nginx
categories:
- django 2.0
---

## 使用 uWSGI 和 Nginx 的原因
Django 是一个 Web 框架，框架的作用在于处理 request 和 reponse，其他的不是框架所关心的内容。所以怎么部署 Django 不是 Django 所需要关心的。

Django 所提供的是一个开发服务器，这个开发服务器，没有经过安全测试，而且使用的是 Python 自带的 simple HTTPServer 创建的，在安全性和效率上都是不行的。

所以不能用 Django 的 Web 服务器直接部署Web服务，所以就有了 django + uWSGI + Nginx 的 Web 服务器搭建方案。

## uWSGI 和 Nginx 介绍
- WSGI
- uWSGI
- uwsgi
- Nginx

**WSGI** 是一种协议，不是任何包不是任何服务器，就和 TCP 协议一样。它定义了 Web 服务器和 Web 应用程序之前如何通信的规范。
注：至于为什么和 Python 扯在一起？因为这个协议是由 Python 在 2003 年提出的。（参考：PEP-333 和 PEP-3333 ）

**uWSGI** 是一个全功能的 HTTP 服务器，他要做的就是把 HTTP 协议转化成语言支持的网络协议。比如把 HTTP 协议转化成 WSGI 协议，让 Python 可以直接使用。

**uwsgi** 是一种 uWSGI 的内部协议，使用二进制方式和其他应用程序进行通信。

**Nginx** 是一个 Web 服务器其中的 HTTP 服务器功能和 uWSGI 功能很类似，但是 Nginx 还可以用作更多用途，比如最常用的反向代理功能。

之间的联系：
**浏览器 `<==HTTP协议==>` Nginx `<==uwsgi协议==>` uWSGI `<==uwsgi协议==> ` Python WSGI Module `<==WSGI协议==>` Python Application**

注：Nginx 主要优化的是连接数和静态文件，uWSGI 主要优化的是 wsgi 服务。uWSGI 实际上可以完成 Nginx 的功能，但 Nginx 更强大，能直接在 Nginx 层面就完成很多事情，比如静态文件、反向代理、转发等需求。

## uWSGI 安装及配置
**1. 安装 uWSGI**
```
$ sudo pip install uwsgi
```
**2. 测试 uWSGI**
创建一个 `test.py` 文件
```
# test.py 文件内容
def application(env, start_response):
    start_response('200 OK', [('Content-Type','text/html')])
    return [b"Hello World"] # python3
    #return ["Hello World"] # python2
```
启动 uWSGI 测试服务
```
uwsgi --http :8000 --wsgi-file test.py
```
浏览器访问 [localhost:8000](localhost:8000)，显示 hello world 表示成功。

**3. 配置 uWSGI**
在工程目录下创建uWSGI的配置文件`/root/test/mysite/uwsgi.ini`
```
# uwsgi.ini 文件内容
[uwsgi]
socket = 127.0.0.1:8000
chdir=/root/test/mysite
module=mysite.wsgi
master = true
workers=2
vacuum=true
thunder-lock=true
enable-threads=true
harakiri=30
post-buffering=4096
daemonize =/root/test/mysite/mysite/uwsgi.log
```
```
# uwsgi.ini 文件说明
[uwsgi]
socket = 127.0.0.1:8000  
chdir=/root/test/mysite  # 工程的绝对路径
module=mysite.wsgi  # wsgi.py在自己工程中的相对路径，”.”指代一层目录
master = true
workers=2
vacuum=true
thunder-lock=true
enable-threads=true
harakiri=30
post-buffering=4096
daemonize =/root/test/mysite/mysite/uwsgi.log  # uWSGI日志的存储路径
```


注：我的 django 项目路径是`/root/test/mysite`
## Nginx 安装及配置
**1. 安装 Nginx**
```
$ sudo apt-get install nginx
```

**2. 配置 Nginx**
定位 Nginx 配置文件路径
```
$ sudo nginx -t
```
会列出以下信息
```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
即 Nginx 默认配置文件的路径是`/etc/nginx/conf/nginx.conf`
打开配置文件，并在 http {} 内添加 server 字段：
```
    server {
        listen 80;
        server_name localhost;
        charset     utf-8;
        access_log      /root/test/mysite/nginx_access.log;
        error_log       /root/test/mysite/nginx_error.log;
        client_max_body_size 75M;


        location /static {
            alias /root/test/mysite/mysite/static;
        }

        location / {
            include     /etc/nginx/uwsgi_params;
            uwsgi_pass  127.0.0.1:8000;
        }
    }
```
```
# server 字段说明
server {
    listen 80;  # 代表服务器开放80端口
    server_name localhost;  # 服务器地址
    charset     utf-8;
    access_log      /root/test/mysite/nginx_access.log;
    error_log       /root/test/mysite/nginx_error.log;
    client_max_body_size 75M;


    location /static {
        alias /root/test/mysite/mysite/static;  # 自己定义的项目引用静态文件
    }

    location / {
        include     /etc/nginx/conf/uwsgi_params;
        uwsgi_pass  127.0.0.1:8000;  # uWSGI绑定的监听地址，这里使用了8000端口
    }
}
```
注：我的 django 项目路径是`/root/test/mysite`

## 启动 uWSGI 与 Nginx
**1. 启动 uWSGI**
```
sudo uwsgi --ini /root/test/mysite/uwsgi.ini
```

**2. 启动 Nginx**
```
nginx -c /etc/nginx/nginx.conf
```
注：-c 表示加载配置文件启动

**3. 你成功了吗**
在浏览器输入 [localhost](localhost) 是不是可以正常访问你的项目服务了。
## 参考资料
- [django 作为 web 服务器为什么线上部署的时候要用到 uwsgi 和 nginx](https://www.v2ex.com/t/375771)
- [uWSGI+django+nginx的工作原理流程与部署历程](http://blog.csdn.net/c465869935/article/details/53242126)
