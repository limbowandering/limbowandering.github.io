---
title: Mac下python3环境安装OpenCV3
date: 2017-09-28
categories: Configuration
tags: [配置, Python, OpenCV]
---

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=29137871&auto=1&height=66"></iframe>

OpenCV很强大, 可是安装过程很苦恼. 再我采坑?次数以后, 总结了下面最简单的安装办法

## Mac安装OpenCV

当然, 要用homebrew来安装.

``` bash
$ brew tap homebrew/science
$ brew install opencv
```

这样下载下来的 应该是OpenCV3.3, 但是下载下来并不能用. 所以要做一个软链接, 首先要进入python3的目录下. 我是用brew安装的python3 所以的目录是

``` bash
$ cd /usr/local/lib/python3.6/site-packages
#下一步是将OpenCV的cv2.cpython-36m-darwin.so文件 软链接到当前目录下面 命名为cv2.so
$ ln -s /usr/local/Cellar/opencv/3.3.0_3/lib/python3.6/site-packages/cv2.cpython-36m-darwin.so cv2.so
```

就是这么简单.

然后就搞定了.

(捂住滴血的心)

### 补充一些操作

```bash
$ vi ~/.bash_profile
#添加一下代码 
# brew 不自动更新
export HOMEBREW_NO_AUTO_UPDATE=true

#第二点: 因为走国内IP实在是太慢了, 所以给终端加个代理, 上网正确姿势自备
#添加一下代码:
#Proxy
alias setproxy="export ALL_PROXY=socks5://127.0.0.1:1086"
alias unsetproxy="unset ALL_PROXY"
alias ip="curl -i http://ip.cn"
#使用方法 setproxy unsetproxy ip

$ source ~/.bash_profile
#end
```

### 一段测试代码

这篇博客太短 所以附上一段OpenCV测试代码

``` python
import numpy as np
import cv2

cap = cv2.VideoCapture(0)

while(True):
    ret, frame = cap.read()
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)


    cv2.imshow('frame', gray)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break


cap.release()
cv2.destroyAllWindows()

```

