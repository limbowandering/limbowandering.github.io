---
title: Windows常用DOS命令
date: 2017-12-21
categories: CS
tags: [Windows]
---

用惯了*nix的系统命令, 一直排斥Windows的命令, 之前使用Linux的命令模拟器在Windows下运行Linux命令, 昨天重装个系统, 装了Ubuntu Windows subsystem, 感觉Windows是有史以来最棒的Linux发行版! 加之最近有了台大屏显示器, 感觉Windows又充满了生机. 于是又要回头折腾折腾. 

与其找blablabla的Linux命令模拟器, 不如就此用Windows的DOS命令吧. そう思う.

## 常用Windows的DOS命令整理

#### 切换目录

从C盘到其他磁盘等切换磁盘的操作, 直接输入磁盘名称:

`D:` 即可

打开文件夹 `cd` 和Linux一样

#### 查看目录下那些文件夹和文件

`dir` 即可. 还有个tree的命令. 挺有意思.

#### 新建文件夹, 文件

`mkdir` 或者`md` 都行. 

创建文件就没有touch那么方便了. 要`type nul>filename.xxx `

同时, 查看文件内容也是`type`

#### 删除文件夹, 文件

`rd` 是空文件夹

`rd /s`是非空文件夹

#### ha

到此忽然发现输入help就可以查看命令…那我干嘛还写这个博客….

完.

## 其他

在wsl中, win的磁盘是挂载在/mnt下面的, 可以在Ubuntu下用命令进行文件转移. いいな.

