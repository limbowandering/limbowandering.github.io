---
title: 使用Hexo搭建博客
date: 2017-07-14 17:19:00
tags: 服务器
categories: Server
---

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=446581154&auto=1&height=66"></iframe>

 小提琴真的是好美.(B站有此小提琴奏见的演奏视频, UP:Zz白日梦zZ.)

第一次用阿里云和hexo搭建博客的时候还挺顺的, 昨天重装个系统后再装hexo, 感觉整个人都崩了. 但是慢慢搞明白了之后,就不那么复杂了. 把搭建过程完整的写在下面,下次就可以直接照着自己的教程搭建了.(估计不会忘了...)

### 搭建hexo

这一步是从重装系统之后,完完全全的裸机开始. 耗时(网速好,15分钟左右)

系统:centOS 6.8 64位

依次安装git nvm nodejs hexo 

``` bash
$ yum install git
$ curl  https://raw.githubusercontent.com/creationix/nvm/v0.13.1/install.sh | bash #这一步是安装nvm
$ nvm list-remote #显示可用的node版本
$ nvm install v6.11.0 #我经历过多少泪之后确定的一个能用的版本
$ nvm alias default v6.11.0#设置默认版本 建议一开始就设置好
#接下来的步骤就是安装hexo了 可以参照官网hexo.io 毕竟一直在更新
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install hexo --save
$ hexo g
$ hexo s
#下面再列出一些nvm的常用操作
$ nvm use 6.11.0 #切换版本
$ nvm list #查看已安装版本
```

至此,hexo应该已经可以运行了,下一部有两种操作,一种是通过nginx修改网站根目录,直接设置在hexo的public下面, 另一种是修改hexo的配置, 让hexo运行在80端口.

### 配置Nginx

安装

```bash
yum install nginx -y
```

然后修改配置,修改 */etc/nginx/conf.d/default.conf*

示例代码:

``` bash
#
# The default server
#

server {
    listen       80;
    #listen       [::]:80 default_server;
    #取消对IPv6地址的监听,centOS6 不支持, 所以.
    server_name  Limbowandering.com;
    root         /blog/public;#根目录

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    location / {
    }

    error_page 404 /404.html;
        location = /blog/public/404.html {
    }

    error_page 500 502 503 504 /50x.html;
        location = /50x.html {
    }

}


```

修改完成后, 启动Nginx

``` bash
$ nginx
$ chkconfig nginx on#设置nginx为开机自动启动
```

至此, 完工.20分钟内妥妥的.

### 第二种方法

感觉这个比nginx麻烦

修改blog文件夹下:_config.yml文件. 手动添加如下代码:

``` bash
server:
  port: 80

#注意, port前面空两格, 80前面空一格.
```

然后运行hexo s的时候就会发现运行在80端口了. 然后就可以浏览器直接访问了.

关掉进程:

``` bash
$ ps -A #查看所有进程
$ Kill -9 2379#举个例子, 如果发现hexo进程是2379的话 就干掉他就好了
```

完工.

### 其他一些小问题:

- 关于win10下占用4000端口的问题 hexo server -p 3600就行了

- 出现以下情况:

  FATAL can not read a block mapping entry; a multiline key may not be an implicit key at line 11, column 9:

  _config.yml文件编辑的时候, 注意选项和值中间有一个空格! 

### 设置一键部署到github pages

首先你得有一个github pages, 这个就不写了.

然后得为hexo安装git插件

``` bash
$ npm install hexo-deployer-git --save
```

然后修改hexo的配置文件:

``` bash
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: 		     https://github.com/limbowandering/limbowandering.github.io
  branch: master
```

然后就是需要一个SSH key. 可直接参考github的说明文档. 

### 设置一键部署到阿里云

如果把博客放在阿里云的话, 还可以设置一键部署 

参考文章: [*阿里云VPS搭建自己的*的Hexo博客](https://www.baidu.com/link?url=TWDPrCiEP_j5STesq06O_j33_8xLCdxp2tx1a1GR3HC8nhsPWBKpajsj5FW_rC6U4jJyP2mMZxKpYonlLEjzya&wd=&eqid=ce08f83800002e4c000000035978451e)

``` bash
$ adduser git#创建git 用户
$ chmod 740 /etc/sudoers
$ vim /etc/sudoers
#找到以下内容
## Allow root to run any commands anywhere
root    ALL=(ALL)     ALL
在下面添加一行:
git ALL=(ALL) ALL
保存退出后改回权限
$ chmod 400 /etc/sudoer
随后设置git用户的密码
$ sudo passwd git
$ su git#切换到git用户 创建文件夹和文件 并赋予相应的权限
$ su git
$ mkdir ~/.ssh
$ vim ~/.ssh/authorized_keys
#然后将电脑中执行 cat ~/.ssh/id_rsa.pub | pbcopy ,将公钥复制粘贴到authorized_keys
$ chmod 600 ~/.ssh/authorzied_keys
$ chmod 700 ~/.ss
然后就可以执行ssh 命令测试是否可以免密登录
$ ssh -v git@SERVER
至此, git用户添加完成

然后, 服务器上建立一个git裸库
创建一个裸仓库，裸仓库就是只保存git信息的Repository, 首先切换到git用户确保git用户拥有仓库所有权
一定要加 --bare，这样才是一个裸库
$ su git
$ cd ~
$ git init --bare blog.git
然后使用git-hooks同步网站根目录
在这里我们使用的是post-receive这个钩子，当git有收发的时候就会调用这个钩子。 在 ~/blog.git 裸库的 hooks文件夹中，
新建post-receive文件.
$ vim ~/blog.git/hooks/post-receive

#!/bin/sh
$ git --work-tree=/path/to/www --git-dir=~/blog.git checkout -f
保存后, 要赋予这个文件可执行权限
$ chmod +x post-receive
最后一步就是配置_config.yml, 完成自动化部署了
deploy:
    type: git
    repo: git@SERVER:/home/git/blog.git    //<repository url>
    branch: master            //这里填写分支   [branch]
    message: 提交的信息         //自定义提交信息 (默认为 Site updated: {{ now('YYYY-MM-DD HH:mm:ss') }})
```



### hexo主题配置

我采用的是[Next主题](http://theme-next.iissnan.com)

这个的话,按照说明文档一步一步来就好了呀~ 感觉这一步是最愉快的! 尽情的美化吧!

OK, 完工. 开始愉快的blog吧.

