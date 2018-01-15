---
title: Hexo博客（三）域名配置
date: 2017-11-21 10:00:21
categories:
- hexo
tags:
- Hexo域名配置
---

## 域名配置
把访问域名git_name.github.io替换成自己的域名。
### 申请个人域名
我是在[万网](https://wanwang.aliyun.com/)申请的，需要进行备案，因为是学习用的，所以申请了.top的域名，首年只要两块钱就可以搞定。
### 添加DNS解析
1. ping初始域名。比如我的Gighub Hexo博客域名为iamsea.github.io
![get_ip.png](http://upload-images.jianshu.io/upload_images/2569188-da68b131de363334.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
2. 进入域名解析设置，添加两条解析，记录值为ping指向的ip
![domain_set.png](http://upload-images.jianshu.io/upload_images/2569188-f7ad2ef37ddbf8a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 添加GNAME文件
进入本地博客项目的source目录，新建GNAME文件，注意GNAME文件没有后缀，文件内容为新的域名。
### Github项目设置
进入Github的博客项目的设置项，修改Custom domain并保存，即会自动生成CNAME文件。ps: 或者进入本地博客项目的source目录，新建CNAME文件，注意CNAME文件没有后缀，文件内容为新的域名。
![custom_domain.png](http://upload-images.jianshu.io/upload_images/2569188-9ff0b6695d01a7bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

配置完成。

## DNS分流
加速博客访问速度
