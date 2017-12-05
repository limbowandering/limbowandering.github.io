---
title: Mac和Windows下配置sublime编译环境
date: 2017-08-06 17:00:00
tags: [配置,Sublime]
categories: Configuration
---

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=1645112&auto=1&height=66"></iframe>

(游戏To the moon的BGM, Johnny给River写的曲子,感动)

sublime真的是太好用了哇. 配置之后真的很方便. 这篇博客就写一下Mac和windows下配置C++编译环境的方法吧.

 [官方文档](http://docs.sublimetext.info/en/latest/file_processing/build_systems.html) 

## windows下配置

[CSDN参考文章](http://blog.csdn.net/ac_machine/article/details/60765491)

步骤:

1. 安装MinGW和添加环境变量
2. 编写build配置文件
3. 完工

这个编译环境什么意思呢? C++的代码在windows下编译运行需要执行一段cmd命令, 而这个命令是通过build配置文件实现的. build配置文件是json格式的, 拿官方文档的例子:

``` json
{
    "cmd": ["python", "-u", "$file"],
    "file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
    "selector": "source.python"
}
```

cmd 后的命令就是 

``` bash
python -u /path/to/current/file.ext
```

的意思咯.

MinGW是什么呢? MinGW是Minimalist GNU on Windows, 里面包含了C++的编译器, 它可以让我们再windows下Linux的命令运行c++程序. 下载安装g++编译器,然后把C:\MinGW\bin添加到环境变量path中.

先把最终代码放出来,直接用这个就行.(Tools->Build System->New一个后保存)

``` json
{  
    "cmd":"g++ -std=c++11 $file_name -o $file_base_name",  
    "file_regex": "^(..[^:]*):([0-9]+):?([0-9]+)?:? (.*)$",  
    "working_dir": "${file_path}",  
    "selector": "source.c, source.c++",  
    "shell": true,  
    "encoding":"cp936",  
    "variants":[  
        {  
            "name":"g++ Compile",  
            "cmd":"g++ -std=c++11 $file_name -o $file_base_name"  
        },  
        {  
            "name":"g++ Run",  
            "cmd":"start cmd /k $file_base_name"  
        }  
    ]  
}  
```

-std=c++11是添加g++的编译选项. 其他的一些命令我也没求甚解.

重点: sublime 的 build system 里接受的命令输出都被重定向到了 sublime 自身， cin 之类的没法接受输入。于是乎上述配置文件的g++ run 会弹出终端,在终端就可以输入了.

配置文件是用json写的,感觉还是比较清晰的:

|     名称      |                    含义                    |
| :---------: | :--------------------------------------: |
| Working_dir |       运行cmd是会切换到working_dir制定的工作目录       |
|     cmd     | 包括命令及其参数.如果不指定绝对路径,外部程序会在你系统的:const:PATH环境变量中搜索. |
|  shell_cmd  |     相当于Shell:True的cmd,cmd可以通过shell运行     |
| file_regex  |  该选项用Perl的正则表达式来捕获构建系统的错误输出到sublime的窗口   |
|  selector   | 在选定Tools Build System Automic时根据这个自动选择编译系统 |
|  variants   |    用来替代主构件系统的备选.例如Run命令,会显示再tool的命令中     |
|    name     |          只在variants下面有,设置命令的名称           |

|       变量        |      含义       |
| :-------------: | :-----------: |
|   $file_path    |  当前文件所在的目录路径  |
|      $file      |   当前文件的详细路径   |
|   $file_name    |  文件全名(包含扩展名)  |
| $file_extension |    当前文件扩展名    |
| $file_base_name | 当前文件名(不包括扩展名) |

一些注意点:

- $file也可以写成${file}.
- 运行cmd: start cmd /k hello . start cmd是在原来的cmd基础上重新打开一个cmd,目录在当前目录. /k的话是运行完不会关闭.
- variants就是备选命令咯,按Ctrl+Shift+B的话会出现对应选项.



## Mac下的配置

[参考配置](http://blog.csdn.net/luminous11/article/details/64129985)

[How to run programs in an external console with Sublime Text?](https://stackoverflow.com/questions/11601423/how-to-run-programs-in-an-external-console-with-sublime-text)

原理在windows中讲过了，在Mac上有不同的是编译器是clang了。所以配置文件有所变化。这里直接给出最后我用的配置：

``` json
{
    "cmd": ["clang++", "${file}","-std=c++11", "-stdlib=libc++", "-o", "${file_path}/${file_base_name}"],
    "file_regex": "^(..[^:]*):([0-9]+):?([0-9]+)?:? (.*)$",
    "working_dir": "${file_path}",
    "selector": "source.c, source.c++",
 
    "variants":
    [
        {
            "name": "Run",
            "cmd": ["bash", "-c", "g++ '${file}' -o '${file_path}/${file_base_name}' && open -a Terminal.app '${file_path}/${file_base_name}'"]
        }
    ]
}
```



完工了。尽情的敲键盘吧。