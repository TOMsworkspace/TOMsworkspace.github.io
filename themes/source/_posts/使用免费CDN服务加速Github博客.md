---
title: 使用免费CDN服务加速Github博客
comments: true
copyright: true
date: 2020-01-15 18:58:14
updated:
tags: 
- 免费CDN服务
- 加速个人博客
- CDN
- github 
- jsdelivr
categories: 其他
keywords: 免费CDN 加速github 个人博客 jsdelivr
description: 
top_img: https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper@1.0/使用免费CDN服务加速Github博客/figure1.jpg
cover: https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper@1.0/使用免费CDN服务加速Github博客/figure1.jpg
toc: true
toc_number: true
---

&emsp;最近发现部署在github上的个人博客网站访问加载的时间越来越久了，本身Github就已经很慢了，这个加载的速度令人难以接受，特别是对于有图片等比较大的文件需要加载的时候。于是开始考虑有没有什么方法能加速一下。

&emsp;最好的方法当然是多花点钱，买个服务器把博客网站部署上去，不过哪有免费的东西用着舒服呢。于是发现了一个叫做CDN的加速方式，可以对资源访问进行加速。其实对于个人站点来说，只要能加速网站上的静态文件，比如图片、js文件、css文件，网站的访问速度就会大大的提升。

## CDN加速的原理

&emsp;CDN的全称是Content Delivery Network，即内容分发网络。CDN是构建在网络之上的内容分发网络，依靠部署在各地的边缘服务器，通过中心平台的负载均衡、内容分发、调度等功能模块，使用户就近获取所需内容，降低网络拥塞，提高用户访问响应速度和命中率。CDN的关键技术主要有内容存储和分发技术。——百度百科

## **七牛云图床**

&emsp;七牛云就是一个这样的工具。可以把你需要加速的图片放到上面去。也提供免费的空间供你使用，不过需要注册和实名认证。感觉不是太友好。你也可以直接把你的博客网站搬上去。原理都是一样的。

&emsp;[配置方法](https://blog.csdn.net/liyifan687/article/details/90312977)

## [jsDelivr](https://www.jsdelivr.com/)

&emsp;jsDelivr的宗旨是为开发者提供免费公共 CDN 加速服务。通过使用jsDelivr,不移动你需要加速的内容,可以直接加速Github项目上的资源。这样可以将个人博客的访问速度大大提升。关键是配置简单，而且完全免费。大力推荐。

![](https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper@1.0/使用免费CDN服务加速Github博客/figure1.jpg)

&emsp;[jsDelivr介绍](https://blog.csdn.net/larpland/article/details/101349605)

&emsp;配置方法

- <https://blog.csdn.net/larpland/article/details/101349605>
- <https://blog.csdn.net/qq_36759224/article/details/86936453>
- <https://www.hojun.cn/2019/01/18/ck4wa261r005jdcvas9gv7evx/>

### 第一步: 发布你的github仓库

&emsp;把你博客里用到的图片放到github仓库里。可以另外建立一个仓库来保存用到的图片，这样方便管理。

&emsp;在仓库界面下，点release发布
![](https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper@1.0/使用免费CDN服务加速Github博客/figure2.jpg)

&emsp;自定义发布版本号

![](https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper@1.0/使用免费CDN服务加速Github博客/figure3.jpg)

### 第二步：在文章中引用你需要的资源

&emsp;使用方法：https://cdn.jsdelivr.net/gh/你的用户名/你的仓库名@发布的版本号/文件路径	
例如：

- https://cdn.jsdelivr.net/gh/TRHX/CDN-for-- itrhx.com@1.0/images/trhx.png
- https://cdn.jsdelivr.net/gh/TRHX/CDN-for-itrhx.com@2.0.1/css/style.css
- https://cdn.jsdelivr.net/gh/moezx/cdn@3.1.3//Sakurasou.mp4

&emsp;注意：版本号不是必需的，是为了区分新旧资源，如果不使用版本号，将会直接引用最新资源，除此之外还可以使用某个范围内的版本，查看所有资源等，具体使用方法如下：	

- // 加载任何Github发布、提交或分支
- https://cdn.jsdelivr.net/gh/user/repo@version/file

- // 加载 jQuery v3.2.1
- https://cdn.jsdelivr.net/gh/jquery/jquery@3.2.1/dist/jquery.min.js

- // 使用版本范围而不是特定版本
- https://cdn.jsdelivr.net/gh/jquery/jquery@3.2/dist/jquery.min.js
- https://cdn.jsdelivr.net/gh/jquery/jquery@3/dist/jquery.min.js

- // 完全省略该版本以获取最新版本
- https://cdn.jsdelivr.net/gh/jquery/jquery/dist/jquery.min.js

- // 将“.min”添加到任何JS/CSS文件中以获取缩小版本，如果不存在，将为会自动生成
- https://cdn.jsdelivr.net/gh/jquery/jquery@3.2.1/src/core.min.js

- // 在末尾添加 / 以获取资源目录列表
- https://cdn.jsdelivr.net/gh/jquery/jquery/

