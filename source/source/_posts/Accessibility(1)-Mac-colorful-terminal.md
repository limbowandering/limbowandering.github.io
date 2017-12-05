---
title: Mac终端配色方案和显示分支以及高亮
date: 2017-08-07 18:00:00
tags: [折腾,Mac,Terminal]
categories: Accessibility
---

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=1645079&auto=1&height=66"></iframe>

(游戏To the moon的BGM, Johnny给River写的曲子)

[参考文章链接](http://www.jianshu.com/p/43613289eb6e) [iterm 让你的命令行丰富多彩](http://blog.jobbole.com/93522/) 

什么？我没有女朋友我折腾一下配色方案看着漂亮不行吗？哼╭(╯^╰)╮.

先放上配好之后的一种方案:

![效果图1](../../../../images/mine/bash1.png)

## 配色

[官网](https://github.com/mbadolato/iTerm2-Color-Schemes) 照着官网的instruction就能做好了,主要步骤如下:

1. 将资源下载到本地解压.
2. 打开iterm preference. 转到profile栏, (新建一个profile)color presets中import一个你看着舒服的.(在解压后的schemes文件夹中. 我发现background图片不错就作为背景图片用上了)
3. 完工

## 显示分支以及高亮

这就看起来很漂亮了

1. 编辑 /etc/profile文件,

   ``` bash
   $ sudo vim /etc/profile
   ```

2. 添加一下代码

   ```bash
   find_git_branch () {

   local dir=. head

   until [ "$dir" -ef / ]; do

   if [ -f "$dir/.git/HEAD" ]; then

   head=$(< "$dir/.git/HEAD")

   if [[ $head = ref:\ refs/heads/* ]]; then

   git_branch=" (${head#*/*/})"

   elif [[ $head != '' ]]; then

   git_branch=" → (detached)"

   else

   git_branch=" → (unknow)"

   fi

   return

   fi

   dir="../$dir"

   done

   git_branch=''

   }

   PROMPT_COMMAND="find_git_branch; $PROMPT_COMMAND"

   black=$'\[\e[1;30m\]'

   red=$'\[\e[1;31m\]'

   green=$'\[\e[1;32m\]'

   yellow=$'\[\e[1;33m\]'

   blue=$'\[\e[1;34m\]'

   magenta=$'\[\e[1;35m\]'

   cyan=$'\[\e[1;36m\]'

   white=$'\[\e[1;37m\]'

   normal=$'\[\e[m\]'

   PS1="$white[$white@$green\h$white:$cyan\W$yellow\$git_branch$white]\$ $normal"

   //以下是出处
   作者：方丈先生
   链接：http://www.jianshu.com/p/43613289eb6e
   來源：简书
   著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
   ```

3. 执行以下代码:

   ```bash
   source /etc/profile
   ```

4. 完工

说明: 这时候再修改配色方法会有更多惊喜等你解锁