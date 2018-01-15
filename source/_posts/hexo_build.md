---
title: Hexo博客（一）搭建
date: 2017-11-14 11:27:10
tags:
- Hexo搭建
categories:
- hexo
---

## Hexo简介与安装

**1. 什么是Hexo？**
Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

**2. 安装**
参照 [官方文档](https://hexo.io/zh-cn/docs/index.html) 完成相关安装步骤

**3. 初始化**
新建一个文件夹，比如blog，作为本地博客文件库
```
# 初始化
$ cd blog
$ hexo init
```
```
# 生成静态界面
$ hexo g（等同于hexo generate）
```
```
# 启动服务
$ hexo s（等同于hexo server）
```
浏览器输入 [http://localhost:4000](http://localhost:4000/) 预览



## 将本地Hexo博客同步到Github
**1. 注册一个Github账号**
这里不做演示

**2. 创建Repository**
Repository的名字即为你博客的域名，命名规则是：`Github用户名.github.io`，比如Github用户名是`iamsea`，那仓库名是`iamsea.github.io`，创建后进入项目设置，会看到
![成功创建博客仓库.png](http://upload-images.jianshu.io/upload_images/2569188-e72c6ee27ec00264.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**3. Github添加SSH key**

```
# 创建一个SSH key
$ ssh-keygen -t rsa -C "your_email@example.com"
```
![生成SSH key.png](http://upload-images.jianshu.io/upload_images/2569188-78d31c96b826a7f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
注: SSH key既是所生成id_rsa文件的全部内容，详细可以参考[这里](http://blog.csdn.net/binyao02123202/article/details/20130891)

添加SSH key至Github
![添加SSH key.png](http://upload-images.jianshu.io/upload_images/2569188-71362e5065ab9b3b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**4. 修改本地站点配置**
打开`blog/_config.yml`文件，既站点配置文件，修改deploy字段
```
deploy:
     type: git
     repo: https://github.com/iamsea/iamsea.github.io.git  #你的博客Repository链接
     branch: master
```

**5. 提交到Gighub**
安装hexo-deployer-git插件
```
$ npm install hexo-deployer-git --save
```
生成静态界面
```
$ hexo g
或
$ hexo generata
```
提交部署到github
```
$ hexo d
或
$ 等同于hexo deploy
```
合并的命令为
```
$ hexo g -d
```
注：看到 `Deploy done: git`表示成功提交

至此，本地Hexo博客已经布署到Github了
浏览器输入`你的Github用户名.github.io`预览

## 参考文章
 - [官方文档](https://hexo.io/zh-cn/docs/index.html)
- [hexo搭建简介](https://zorpz.github.io/blog/hexo%E6%90%AD%E5%BB%BA%E7%AE%80%E4%BB%8B.html)
