---
title: CentOS,Ubuntu安装python3+Flask
date: 2017-10-19
categories: Linux
tags: [Python, Flask, Linux, Ubuntu,CentOS]
---

上次在Linux上编译安装了python3后手贱替换了系统默认python. 于是乎yum崩了. 于是乎重装系统. 先重新安装python3. 再安装flask吧.

## Linux安装python3

``` bash
$ yum groupinstall 'Development Tools'
$ yum install zlib-devel bzip2-devel openssl-devel ncurese-devel

$ wget ....(自行找到tar的)
$ tar -xvzf Python....
$ cd Python...
$ ./configure --prefix=/usr/local/python3 #指定安装目录
$ sudo make
$ sudo make install
#接下来作死的心又有了. 既然只是yum以来python2,那就之后再修改yum配置吧
#下面还是修改默认为python3
#备份
$ sudo mv python python.bak
$ sudo ln -s /usr/local/python3/bin/python3 /usr/bin/python
#这样就替换了
# 更换yum配置文件
$ sudo vi /usr/bin/yum
将第一行指定的python版本改为python2.7
#!/usr/bin/python2.7
```



## CentOS,Ubuntu安装flask

感觉自己写这个博客并没有大用处, flask的文档写的真是太好了. 回头就去度文档了.

``` bash
sudo easy_install virtualenv
mkdir Flask
cd Flask
virtualenv venv
source venv/bin/activate
#deactivate
pip install Flask
```

好像是我遇到过最简单的安装了.

