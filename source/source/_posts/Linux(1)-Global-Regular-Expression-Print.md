---
title: Linux grep命令
date: 2017-10-03 20:00:20
categories: Linux
tags: [Linux]
---

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=139177&auto=1&height=66"></iframe>

## Linux grep命令

### 作用

Linux系统中grep命令是一种强大的文本搜索工具, 它能使用正则表达式搜索文本, 并把匹配的行打印出来. grep全称是Global Regular Expression Print, 表示全局正则表达式版本, 它的使用权限是所有用户.

### 格式

` grep [options] `

### 主要参数

`man grep` 就行了啊.

`-c:` 只输出匹配行的计数.

`-I:` 不区分大小写, 只适用于单字符.

`-h:` 查询多文件时不显示文件名.

`-l:` 查询多文件时候只输出包含匹配字符的文件名.

`-n:` 显示匹配行及行号.

`-s:` 不显示不存在或无匹配文本的文件名

`-v:` 显示不包含匹配文本的所有行

pattern正则表达式主要参数:

`\:` 忽略正则表达式中特殊字符的原有含义。

`^:` 匹配正则表达式的开始行

`$:` 匹配正则表达式的结束行

\`\<:` 从匹配正则表达式的行开始

`\\>:`到匹配正则表达式的行结束

`[ ] :` 单个字符

`[ - ] :` 范围

`.:` 所有的单个字符

`*:` 有字符, 长度可以为0

### grep命令使用实例

``` bash
$ grep 'test' d*#显示所有以d开头的文件中包含test的行
$ grep 'test' aa bb cc#显示在aa,bb,cc文件中匹配test的行
$ grep '[a-z]\{5\}' aa#显示在aa中所有至少有5个连续小写字符的字符串的行
$ grep 'W\(es\)t.*\1' aa
$ grep -c "48" data.doc #输出文档中含有48字符的行数
$ grep -n "48" data.doc #显示所有匹配48的行和行号
$ grep -vn "48" data.doc #输出所有不包含48的行
$ grep -i "ab" data.doc #输出所有含有ab或AB的字符串的行
$ grep '[239].' data.doc #输出所有含有以2,3,或9开头的,并且是两个数字的行
$ grep '^[^48]' data.doc #不匹配行首是48的行
$ ...
```

附一些可以使用国际模式匹配的类名:

| [[:upper:]] |     [A-Z]     |
| :---------: | :-----------: |
| [[:lower:]] |     [a-z]     |
| [[:digit:]] |     [0-9]     |
| [[:alnum:]] | [0-9,a-z.A-Z] |
| [[:space:]] |    空格或者tab    |
| [[:alpha:]] |   [a-z,A-Z]   |

