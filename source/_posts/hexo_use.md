---
title: Hexo博客（二）配置
date: 2017-11-16 11:27:10
tags:
- Hexo搭建
categories:
- hexo
---

## 站点基本配置
站点配置文件_config.yml
```
title: 王者峡谷	# 博客标题
subtitle:	# 博客副标题
description: 见过我家那只可爱的宠物吗	# 作者描述
author: Lunar	# 作者名
language: zh-Hans	# 博客语言
timezone:
```

## 修改主题
1. 下载主题，将主题文件夹放入`blog/themes`文件夹
2. 修改 `blog/_config.yml` 文件里面的 `theme` 字段为新的主题文件夹名
3. 如果该主题有相关依赖插件，记得安装
4. `hexo g -d`提交
## 修改导航栏
这里以[next](https://github.com/iissnan/hexo-theme-next)主题为例
初始导航栏只有首页跟归档
1. 打开`blog/themes/next/_config.yml`主题配置文件
2. 修改menu字段，取消about(关于)及tags(标签)的注释
```
menu:
  home: / || home
  about: /about/ || user
  tags: /tags/ || tags
  #categories: /categories/ || th
  archives: /archives/ || archive
  #schedule: /schedule/ || calendar
  #sitemap: /sitemap.xml || sitemap
  #commonweal: /404/ || heartbeat
```
3. 修改后博客会多出对应的导航项，但导航页面要另外添加

## 添加导航页面
**1. 添加关于导航页**
```
$ cd blog
$ hexo new page about
```
此时会在source目录下生about目录
打开`blog/source/about/index.md`文件
```
---
title: about
date: 2017-11-15 15:02:22
---
```
作如下修改
```
---
title: 关于
date: 2017-11-15 15:02:22
---
这是关于页。。。

```
**2. 添加标签导航页**
```
$ hexo new page tags
```
此时会在source目录下生tags目录
打开`blog/source/tags/index.md`文件
```
---
title: tags
date: 2017-11-15 12:25:21
---
```
作如下修改
```
---
title: 标签
date: 2017-11-15 12:25:21
type: "tags"
---
```
**3. 添加分类导航页面**
```
$ hexo new page categories
```
此时会在source目录下生categories目录
打开`blog/source/categories/index.md`文件
作同样修改
```
---
title: 分类
date: 2017-11-15 14:38:58
type: "categories"
---
```

## 新建博客
比如新建一篇博文叫《Hexo安装》
```
$ hexo new  hexo-install
```
此时会在source目录下的_posts目录生成hexo-install.md文件
打开这个文件
```
---
title: hexo_install
date: 2017-11-16 15:04:29
tags:
---
```
进行如下编辑
```
---
title: Hexo安装
date: 2017-11-16 15:04:29
tags: Hexo
categories: Hexo
---
博文内容...
```
`title: Hexo安装`是博文标题
`tags: Hexo`是标签信息
`categories: Hexo`是分类信息

## 添加用户头像
将头像图片(eg: avatar.png)放在next主题source/images目录下
取消主题配置文件_config.yml下的avatar字段的注释
```
avatar: /images/avatar.png
```

## 博文中文乱码问题
用记事本打开.md文件并另存为UTF-8格式即可
