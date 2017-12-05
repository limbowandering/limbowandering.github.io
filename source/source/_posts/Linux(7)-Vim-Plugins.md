---
title: Vim配置与练级
date: 2017-10-20 12:00:00
categories: Linux
tags: [Vim,Linux]
---

Vim, 这个编辑器. 是很有用的. 配置好了, 也是极棒的.

目前我是只会最简单的编辑(属于存活阶段的.)下面要学一些常用的快捷键. 今天写博客是配了好多插件. 记录一下配置, 慢慢学习.

## Vim插件配置

配置文件我已经放到我博客了. 可以点击[这里](https://limbowandering.github.io/docs/vimrc_bak) 

(Mac的配置方法)

然后将这个文件放到home目录下. 即可. 接下来就是下载bundle:

``` bash
$ git clone https://github.com/VundleVim/Vundle.vim.git
```

然后进入vim

输入`PluginInstall`

漫长的等待. 

而后YouCompleteMe的插件需要编译一下. 参照:https://github.com/Valloric/YouCompleteMe#mac-os-x

之后可能还有一些各种小问题. 比如我的主题找不到了. 就手动建立一个目录.`~/.vim/colors/` 然后去github上手动下载主题.vim文件. 手动放进去就好了

## Vim插件使用技巧

### NERD Tree

NERD Tree 是一个树形目录插件, 方便浏览当前目录有哪些目录和文件. 

在~/.vimrc文件中配置NERD Tree, 设置一个启用或禁用NERD Tree的键映射

`nmap <F5> : NERDTreeToggle<cr>`

然后按F5就能启用或禁用NERD Tree. NERD Tree提供一些常用快捷键来操作目录.

- 用过hjkl移动光标.
- o打开关闭文件或目录.
- t在标签页中打开
- s和i可以水平或纵向分割窗口打开文件
- p到上层目录
- P到根目录
- K到同目录第一个节点
- P到同目录最后一个节点

### YouCompleteMe&syntastic

YouCompleteMe是一个快速, 支持模糊匹配的vim代码补全引擎. 而且集成了Syntastic, 所以代码有语法错误就会有红色错误提示.

## Vim练级攻略

[Vim练级攻略](http://yannesposito.com/Scratch/en/blog/Learn-Vim-Progressively/) 

[简明Vim练级攻略](https://coolshell.cn/articles/5426.html) 

## iterm2快捷键

一个软件的快捷键会用了, 其他的只要查一下也会用了.

iterm2是Mac下的命令行工具.

|    新建标签     |     Command+T      |
| :---------: | :----------------: |
|    关闭标签     |     Command+W      |
|    切换标签     | Command+数字/Command |
|    切换全屏     |   Command+Enter    |
|     查找      |     Command+F      |
|    垂直分屏     |     Command+D      |
|    水平分屏     |  Command+Shift+D   |
|    切换屏幕     | Command+Option+方向键 |
|   查看剪贴板历史   |  Command+Shift+H   |
|    清除当前行    |       Ctrl+U       |
|     到行首     |       Ctrl+A       |
|     到行尾     |       Ctrl+e       |
|    前进/后退    |      Ctrl+f/b      |
|    上一条命令    |       Ctrl+P       |
|   搜索命令历史    |       Ctrl+R       |
|  删除当前光标的字符  |       Ctrl+d       |
| 删除当前光标之前的字符 |       Ctrl+H       |
|  删除光标之前的单词  |       Ctrl+W       |
|   删除到文本末尾   |       Ctrl+K       |
|     清屏      |       Ctrl+L       |

