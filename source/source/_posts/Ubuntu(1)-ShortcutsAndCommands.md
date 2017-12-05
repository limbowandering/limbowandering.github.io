---
title: Ubuntu快捷键以及基础上手命令
date: 2017-10-07 15:21:00
categories: Ubuntu
tags: [Ubuntu, 折腾]
---

## 一些快捷键

Ubuntu下面的软件直接终端输入名称就可以运行. 也挺不错的.

|      操作      |          效果          |
| :----------: | :------------------: |
|    ALT+F1    | 聚焦到桌面左侧任务导航栏, 上下左右导航 |
|    ALT+F2    |         运行命令         |
|    ALT+F4    |         关闭窗口         |
|   ALT+TAB    |         切换程序         |
|  ALT+SPACE   |        打开窗口菜单        |
|  PRINTSCRN   |         截全屏          |
|  ALT+PRINT   |        截取当前窗口        |
|  ALT+PRINT   |       截取任意矩形内容       |
|    SUPER     |        打开Dash        |
|   SUPER+A    |       搜索或浏览程序        |
|   SUPER+F    |       搜索或浏览文件        |
|   SUPER+M    |       搜索或浏览文件        |
| ALT+[1 - 8]  |    Firefox跳到指定标签页    |
|    ALT+9     |   Firefox跳到最后一个标签页   |
|  CTRL+ALT+T  |         打开终端         |
| CTRL+SHIFT+V |        终端内粘贴         |
| SUPER+[1-9]  |         快速运行         |

一些终端的快捷键

| CTRL+A |           光标移动到开始位置            |
| :----: | :----------------------------: |
| CTRL+E |            光标移动到末尾             |
| CTRL+K |            删除到末尾内容             |
| CTRL+U |           删除到开头所有内容            |
| CTRL+H |          删除当前字符前一个字符           |
| CTRL+D |             删除当前字符             |
| CTRL+W |             删除左边单词             |
| CTRL+Y | 粘贴由Ctrl+u， Ctrl+d， Ctrl+w删除的单词 |
| CTRL+L |             clear              |
| CTRL+R |             查找历史命令             |

## aptitude

> aptitude与 apt-get 一样，是 Debian 及其衍生系统中功能极其强大的包管理工具基于大名鼎鼎的APT机制, 整合了 dselect 和 apt-get 的所有功能, 并提供的更多特性,特别是在依赖关系处理上。。与 apt-get 不同的是，aptitude在处理依赖问题上更佳一些。举例来说，aptitude在删除一个包时，会同时删除本身所依赖的包。这样，系统中不会残留无用的包，整个系统更为干净。

``` bash
安装
sudo apt install aptitude

语法:
aptitude 选项 参数

选项:
-h：显示帮助信息
-d：仅下载软件包，不执行安装操作
-P：每一步操作都要求确认
-y：所有问题都回答“yes”
-v：显示附加信息； 
-u：启动时下载新的软件包列表

aptitude update            更新可用的包列表
aptitude safe-upgrade      执行一次安全的升级
aptitude full-upgrade      将系统升级到新的发行版
aptitude install pkgname   安装包
aptitude remove pkgname    删除包
aptitude purge pkgname     删除包及其配置文件
aptitude search string     搜索包
aptitude show pkgname      显示包的详细信息
aptitude clean             删除下载的包文件
aptitude autoclean         仅删除过期的包文件
```

## Shutter

安装: `sudo apt-get install shutter` 

添加快捷键: 打开系统设置->键盘->快捷键->Custom Shortcuts.

名称: Shutter Select. 命令: shutter -s. 快捷键: CTRL+ALT+A(和QQ一样 习惯了)

## Docky

`sudo apt install docky`

## 安装搜狗输入法

1. 官网下载
2. `sudo apt-get install -f`
3. `sudo dpkg -i sogouXXXXX(文件名)`
4. 完成(可能需要重启)

## 更改grub启动背景

把想作为背景的图片放到/boot/grub下面

然后执行`sudo update-grub`

(一开机就是自己想看的~美滋滋)

## 安装视频播放器

选择MPV

``` bash
$ sudo add-apt-repository ppa:mc3man/mpv-tests
$ sudo apt-get update 
$ sudo apt-get install mpv
```

 完毕.

## 其他

更换扁平化主题: Libra

更换扁平化图标: Flat

安装其他程序: Albert, Zim, Redshift,mpv,……..

什么, 你还想听音乐? 你是编程的还是玩Ubuntu的>.< 不会用手机吗!

从此调入Ubuntu坑, 万劫不复.

完.



