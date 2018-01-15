---
title: django 2.0（一）安装与连接MySQL
date: 2017-12-09 17:00:31
tags:
- django连接mysql
categories:
- django 2.0
---

## 安装
django2.0不支持python2.x，所以安装python3.x。
```
# linux安装python3
$ sudo apt-get install python3
```
```
mac安装python3
$ brew install python3
```
安装django 2.0：
```
$ pip3 install django==2.0
```
## 站点初始化
**1. 初始化**
```
$ django-admin startproject mysite 
```
```
主要文件结构：
mysite/
├── db.sqlite3  # 自带数据库文件
├── manage.py   # 项目管理文件
└── mysite
    ├── __init__.py    
    ├── settings.py  # 全局设置文件
    ├── urls.py      # 全局路由控制
    └── wsgi.py      # 服务器部署文件
```
**2. 站点预览**
```
$ cd mysite
$ python3 manage.py runserver
```
注：runserver后面可指定ip及端口号，如下
```
$ python3 manage.py runserver 127.0.0.1:8000
```
## 连接MySQL数据库
**1. 安装django与mysql的连接工具mysqlclient**
```
$ sudo pip3 install mysqlclient
```
注：若使用3.6以下版本的python，则须使用pymysql代替mysqlclient
```
$ sudo pip3 install pymysql
```
并在`mysite/mysite/__init__.py`文件加入以下代码：
```
import pymysql
pymysql.install_as_MySQLdb()
```
**2. 配置数据库信息**
修改`mysite/mysite/settings.py`文件代码：
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'test', 
        'USER': 'root',
        'PASSWORD': '123456',
        'HOST':'localhost',
        'PORT':'3306',
    }
}
```
**3. 新建app**
```
$ django-admin startapp TestModel
```
**4. 修改`mysite/TestModel/models.py`文件代码（定义模型）：**
```
from django.db import models
 
class Test(models.Model):
    name = models.CharField(max_length=20)
```
**5. 修改`mysite/mysite/settings.py`文件代码：**
```
INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'TestModel',  # 添加此项
)
```
**6. 迁移表结构到数据**
```
$ python3 manage.py makemigrations  # 制造迁移
$ python3 manage.py migrate  # 迁移
```
注：若提示`django.db.utils.InternalError: (1049, "Unknown database 'test'")`，则需先手动在mysql创建test数据库

## 数据库操作
**1. 添加数据**
添加`mysite/mysite/testdb.py`文件，内容如下：
```
# -*- coding: utf-8 -*-
 
from django.http import HttpResponse
 
from TestModel.models import Test
 
# 数据库操作
def testdb(request):
    test1 = Test(name='luna')
    test1.save()
    return HttpResponse("<p>数据添加成功！</p>")
```
修改`mysite/mysite/urls.py`文件代码：
```
from django.contrib import admin
from django.urls import path

from django.conf.urls import *
from . import testdb

urlpatterns = [
    path('admin/', admin.site.urls),
    url(r'^testdb$', testdb.testdb),
]
```
访问 [http://127.0.0.1:8000/testdb](http://127.0.0.1:8000/testdb) 即可将数据添加到数据库。
注：若出现utf-8编码问题，用记事本将文件转utf-8编码格式即可。
**2. 删除数据**
修改`mysite/mysite/testdb.py`文件代码：
```
# -*- coding: utf-8 -*-
 
from django.http import HttpResponse
 
from TestModel.models import Test
 
# 数据库操作
def testdb(request):
    # 删除id=1的数据
    test1 = Test.objects.get(id=1)
    test1.delete()
    
    # 另外一种方式
    # Test.objects.filter(id=1).delete()
    
    # 删除所有数据
    # Test.objects.all().delete()
    
    return HttpResponse("<p>删除成功</p>")
```
访问 [http://127.0.0.1:8000/testdb](http://127.0.0.1:8000/testdb) 即可将数据从数据库删除。

## 参考文章
- [Django 模型](http://www.runoob.com/django/django-model.html)