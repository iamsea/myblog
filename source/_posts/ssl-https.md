---
title: Cloudflare 免费SSL证书使用
date: 2017-11-28 17:29:08
tags:
- Cloudflare
- Hexo站点https
- 网络
categories:
- hexo
---
这段时间用Hexo框架搭建了个博客，并部署到了Github，然后重定向到自己的域名，至此网站使用的是http。那么问题来了，如何启动网站的https？

## 工具
[Cloudflare](https://www.cloudflare.com/)
Cloudflare以向客户提供网站安全管理、性能优化及相关的技术支持为主要业务。我们这里使用全站Cloudflare的免费SSL证书实现全站SSL加密（即https）访问。

## 创建Cloudflare账户并添加网站
[注册](https://www.cloudflare.com/a/sign-up)并登录后，输入你的网站进行扫描域名解析。
![image.png](http://upload-images.jianshu.io/upload_images/2569188-82d3ab95381fdca4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##  域名解析
如果提示没有正常扫描域名解析，则需要手动点击Add Record添加。
![域名解析.png](http://upload-images.jianshu.io/upload_images/2569188-022250c87a763b53.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 域名服务器
选择免费服务后会得到两个域名服务器。
![免费服务.png](http://upload-images.jianshu.io/upload_images/2569188-9ef1318e701642ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![DNS.png](http://upload-images.jianshu.io/upload_images/2569188-23104c1e67e514f4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 修改所使用域名的DNS
我这里使用的是阿里[万网](https://wanwang.aliyun.com/)的域名，进入域名管理进行DNS修改就可以了。
![域名DNS.png](http://upload-images.jianshu.io/upload_images/2569188-c53b1e4cf7f3d75d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## Cloudflare站点配置
### Overview界面，检查域名DNS
点击继续，然后点击 Recheck Nameservers检查域名DNS是否已修改成功，这里可能有些时间延迟，成功后会提示Status: Active。
![Overview.png](http://upload-images.jianshu.io/upload_images/2569188-4c382823f7a519d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![Overview.png](http://upload-images.jianshu.io/upload_images/2569188-cf6a9a03a08fbd19.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Crypto界面，配置SSL加密
选择Flexible加密方式，加密方式区别如下。
```
Flexible SSL：您的网站访问者和Cloudflare之间有加密连接，但是从Cloudflare到您的服务器没有加密。即半程加密。优点在于：你的网站不需要SSL证书，用户也能实现SSL加密访问。
Full SSL：全程加密，即从你的网站到CDN服务器再到用户，全程都是SSL加密的。优点在于：只要你的服务器有SSL证书（不管是自签名证书还是购买的SSL），就可以实现SSL加密访问。
Full SSL (strict)：全程加密，它与Full SSL的区别在于你的服务器必须是安装了那些已经受信任的SSL证书（即购买的SSL证书），否则无法开启SSL加密访问。
Strict (SSL-Only Origin Pull)：企业模式。自动将所有的Http转化为Https加密访问，要求你的服务器安装了受信任的有效的SSL证书。
```
![Crypto.png](http://upload-images.jianshu.io/upload_images/2569188-019b57fe4e2f70ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Page Rules界面，页面重定向
以iamsea.top这个域名为例。
#### 二级域名重定向
二级域名www.iamsea.top重定向到一级域名https://iamsea.top。
![Page Rules.png](http://upload-images.jianshu.io/upload_images/2569188-b75304909b8bee44.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### 将http访问转为https访问
![Page Rules.png](http://upload-images.jianshu.io/upload_images/2569188-e82d8d3353d1cfb8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Page Rules.png](http://upload-images.jianshu.io/upload_images/2569188-e55cd81b7d1c9099.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

配置生效可能有时间延迟，以上。

## 参考文章
- [为自定义域名的GitHub Pages添加SSL 完整方案](https://segmentfault.com/a/1190000007740693)
- [新Cloudflare免费CDN加速和Cloudflare免费SSL证书服务申请与使用](https://www.freehao123.com/cloudflare-cdn-ssl/)
- [利用 cloudflare flexible ssl实现wordpress全站强制https](https://zvv.me/z/1032.html)
