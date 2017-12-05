---
title: Linux下编译安装python3
date: 2017-09-25
categories: Linux
tags: [Linux, Python]
---

Linux下默认系统自带python2.6的版本.

``` bash
[root@VM_19_0_centos ~]# python
Python 2.6.6 (r266:84292, Aug 18 2016, 15:13:37)
[GCC 4.4.7 20120313 (Red Hat 4.4.7-17)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>>
//如上 (腾讯云Centos)
```

这个版本不建议删除, 下面做一下编译安装python3.

## 编译安装

首先去python官网下载python3的源码包.

``` bash
$ wget https://www.python.org/ftp/python/3.3.7/Python-3.3.7.tgz

//释放文件
$  tar -xvzf Python-3.3.7.tgz

//进入目录
$ cd Python-3.3.7

//添加配置
$ ./configure --prefix=/usr/python

//编译源码
$ make

//执行安装
$ make install
```

整个过程大约5-10分钟, 安装成功之后, 安装目录就在`/usr/python`

系统中原来的python在 `/usr/bin/python` , 通过` ls -l ` 可以看到, python是一个软链接. 链接到本目录下的python2.6

``` bash
-rwxr-xr-x    2 root root       4864 Aug 18  2016 python
lrwxrwxrwx    1 root root          6 Nov 10  2016 python2 -> python
-rwxr-xr-x    2 root root       4864 Aug 18  2016 python2.6
```

我们可以把这个删除, 也可以新建一个python3的软链接, 只不过执行时python要改成python3, 或者python脚步头部声明要改为#!/usr/bin/python3

这里为了方便建议先重命名一下, 然后建立个软链接就可以了, 之前的程序头部也不用改:

``` bash
[root@VM_19_0_centos ~]# mv /usr/bin/python /usr/bin/python.bak
[root@VM_19_0_centos ~]# ln -s /usr/python/bin/python3 /usr/bin/python
[root@VM_19_0_centos ~]# python
Python 3.3.7 (default, Oct  2 2017, 11:00:07)
[GCC 4.4.7 20120313 (Red Hat 4.4.7-18)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> exit()
[root@VM_19_0_centos ~]#
[root@VM_19_0_centos ~]# python2.6
Python 2.6.6 (r266:84292, Aug 18 2016, 15:13:37)
[GCC 4.4.7 20120313 (Red Hat 4.4.7-17)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

这样就建立好了, 以后直接执行python命令, 就相当于调用python3,实际上python3也是个软链接, 链接到python3.5.1, 这个多次链接其实不影响. 输入python2.6 还会有python2.6的.

