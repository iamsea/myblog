---
title: Hexo博客（四）添加Gitment评论系统
date: 2017-11-21 21:37:56
tags:
- Hexo添加Gitment
categories:
- hexo
---

本教程基于hexo博客next主题

## 注册 OAuth Application
[点击此处](https://github.com/settings/applications/new) 来注册一个新的 OAuth Application。其他内容可以随意填写，但要确保填入正确的 `callback URL`（一般是博客的域名，如Hexo博客部署到Github, 生成域名`http://iamsea.github.io`，最终解析到`http://iamsea.top`，则此时应该填入的是`http://iamsea.top`），注册成功后生成`client_id`及`client_secret`。
![oauth_application.png](http://upload-images.jianshu.io/upload_images/2569188-badee872d96878bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 引入Gitment
在博客目录下执行
```
$ npm install --save gitment
```
打开主题配置文件_config.yml
```
gitment:
  enable: true # 是否开启gitment评论系统
  mint: true #
  count: true # 是否显示评论数
  lazy: true # 懒加载，设置为ture时需手动展开评论
  cleanly: true # 是否隐藏'Powered by ...'
  language: en # 语言，置空则随主题的语言
  github_user: iamsea # Github用户名
  github_repo: comment # 在Github新建一个仓库用于存放评论，这是仓库名
  client_id: a6df579b14f7da8ed99c # 注册OAuth Application时生成
  client_secret: 1f6568974d6f3ed28055d2243d05457f7e04cfd3 # 注册OAuth Application时生成
  proxy_gateway: # Address of api proxy, See: https://github.com/aimingoo/intersect
  redirect_protocol: # Protocol of redirect_uri with force_redirect_protocol when mint enabled
```

## 初始化评论
页面发布后，你需要访问页面并使用你的 GitHub 账号登录（请确保你的账号是第二步所填 repo 的 owner），点击初始化按钮。

## 参考
[Gitment：使用 GitHub Issues 搭建评论系统](https://imsun.net/posts/gitment-introduction/)
[Gitment配置相关问题](https://github.com/imsun/gitment/issues)
