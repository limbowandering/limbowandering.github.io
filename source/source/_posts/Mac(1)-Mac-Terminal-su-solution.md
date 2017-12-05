---
title: Mac终端su命令提示sorry的解决办法
date: 2017–08-07 22:00:00
tags: [Mac,Terminal]
categories: Mac
---

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=1645079&auto=1&height=66"></iframe>

(游戏To the moon的BGM, Johnny给River写的曲子)

刚才用电脑想切换到root用户的时候发现密码有问题, 是我忘了?(不可能)还是有问题. 于是乎...换个密码就好了.

打开终端:

```bash
$ sudo su
```

输入密码(当前用户登录密码) 回车 接着

```bash
passwd root
```

Changing password for root

两次输入密码之后就修改了root用户的密码了.

Solved.