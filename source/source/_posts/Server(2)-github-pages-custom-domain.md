---
title: github pages custom domain
date: 2017-07-26 17:50:00
tags: 服务器
categories: Server
---

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=28786018&auto=1&height=66"></iframe>

来世愿住樱花庄.

正题: 再捣鼓了阿里云腾讯云之后, 还是感觉直接放github上好, 就记录一下怎么弄吧.

### github pages

[第一篇博客](http://blog.limbowandering.com/2017/07/14/%E4%BD%BF%E7%94%A8Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/)就写过怎么弄了, 这里接着上次完成的步骤写.

在本地博客的public文件夹下, 建立一个CNAME文件. 注意, 全大写, 无后缀名. 里头写上你要绑定的网址:

``` bash
blog.limbowandering.com
```

然后部署到github上去

### 添加解析

到域名管理里头去, 添加解析:

CNAME记录：如果将域名指向一个域名，实现与被指向域名相同的访问效果，需要增加CNAME记录.

我只要把blog.limbowandering.com解析到github就行了, 所以主机记录是blog. 记录值就填写 limbowandering.github.io

然后去github上看看, 看到

Your site is published at http://blog.limbowandering.com

就完工了.

感觉简单到不值得写一篇博客.

可以ping 一下自己的网址看看是不是github的服务器地址

