---
title: Terminal-Mac-cheatsheet
date: 2017-08-17 20:00:00
tags: [Mac,Terminal,Linux]
categories: Mac
---

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=28892385&auto=1&height=66"></iframe>

Mac终端命令备忘清单.转载自github: [Terminal-Mac-che'atsheet](https://github.com/durul/terminal-mac-cheatsheet)

## 快捷键

| 按键/命令       | 描述                        |
| ----------- | ------------------------- |
| Ctrl + A    | 移动光标至行首                   |
| Ctrl + E    | 移动光标至行尾                   |
| Ctrl + L    | 清屏                        |
| Command + K | 清屏                        |
| Ctrl + U    | 删除光标前的所有文字。如果光标位于行尾则删除整行。 |
| Ctrl + H    | 与退格键相同                    |
| Ctrl + R    | 检索使用过的命令                  |
| Ctrl + C    | 终止当前执行                    |
| Ctrl + D    | 退出当前shell                 |
| Ctrl + Z    | 将执行中的任何东西放入后台进程。fg可以将其恢复。 |
| Ctrl + W    | 删除光标之前的单词                 |
| Ctrl + K    | 删除光标后的所有文字                |
| Ctrl + T    | 将光标前的两个文字进行互换             |
| Ctrl + F    | 光标向前移动一个单词                |
| Ctrl + B    | 光标向后移动一个单词                |
| Esc + T     | 将光标前的两个单词进行互换             |
| Tab         | 自动补全文件或文件夹的名称             |

## 核心命令

| 按键/命令          | 描述                        |
| -------------- | ------------------------- |
| cd             | Home目录                    |
| cd [folder]    | 切换目录                      |
| cd ~           | Home目录, 例如 'cd ~/folder/' |
| cd /           | 根目录                       |
| ls             | 文件列表                      |
| ls -l          | 文件详细列表                    |
| ls -a          | 列出隐藏文件                    |
| ls -lh         | 文件详细列表中的文件大小以更友好的形式列出     |
| ls -R          | 递归显示文件夹中的内容               |
| sudo [command] | 以超级用户身份执行命令               |
| open [file]    | 打开文件 ( 相当于双击一个文件 )        |
| top            | 显示运行中的进程，按q终止             |
| nano [file]    | 打开编辑                      |
| pico	[file]    | 打开编辑                      |
| q              | 退出                        |
| clear          | 清屏                        |

## 命令历史

| 按键/命令     | 描述                |
| --------- | ----------------- |
| history n | 列出最近执行过的n条命令      |
| ctrl-r    | 检索之前执行过的命令        |
| ![value]  | 执行最近以'value'开始的命令 |
| !!        | 执行最近执行过的命令        |

## 文件管理

| 按键/命令                    | 描述                            |
| ------------------------ | ----------------------------- |
| touch [file]             | 创建一个新文件                       |
| pwd                      | 显示当前工作目录                      |
| ..                       | 上级目录, 例如.                     |
|                          | 'ls -l ..' = 上级目录的文件详细列表      |
|                          | 'cd ../../' = 向上移动两个层级        |
| .                        | 当前目录                          |
| cat                      | 连接                            |
| rm [file]                | 移除文件, 例如 rm [file] [file]     |
| rm -i [file]             | 移除时出现确认提示                     |
| rm -r [dir]              | 移除文件及内容                       |
| rm -f [file]             | 强制移除                          |
| cp [file] [newfile]      | 复制文件                          |
| cp [file] [dir]          | 复制文件到指定目录                     |
| mv [file] [new filename] | 移动/重命名, 例如 mv -v [file] [dir] |

## 目录管理

| 按键/命令                | 描述                  |
| -------------------- | ------------------- |
| mkdir [dir]          | 创建新目录               |
| mkdir -p [dir]/[dir] | 创建子目录               |
| rmdir [dir]          | 移除目录 ( 仅限目录下没有内容时 ) |
| rm -R [dir]          | 移除目录及内容             |

## 管道-连接多个带有输出的命令

| 按键/命令     | 描述                |
| --------- | ----------------- |
| more      | 按当前窗口大小输出内容       |
| > [file]  | 输出至指定文件, 注意文件将会覆盖 |
| >> [file] | 在制定文件的末尾附加内容      |
| <         | 从文件中读取内容          |

## 帮助

| 按键/命令            | 描述          |
| ---------------- | ----------- |
| [command] -h     | 显示帮助信息      |
| [command] --help | 显示帮助信息      |
| [command] help   | 显示帮助信息      |
| reset            | 重置当前终端      |
| man [command]    | 显示指定命令的帮助信息 |
| whatis [command] | 显示指定命令的简述   |

## English Version

### SHORTCUTS

| Key/Command | Description                              |
| ----------- | ---------------------------------------- |
| Ctrl + A    | Go to the beginning of the line you are currently typing on. This also works for most text input fields system wide. Netbeans being one exception |
| Ctrl + E    | Go to the end of the line you are currently typing on. This also works for most text input fields system wide. Netbeans being one exception |
| Ctrl + L    | Clears the Screen                        |
| ⌘Cmd + K    | Clears the Screen                        |
| Ctrl + U    | Cut everything backwards to beginning of line |
| Ctrl + K    | Cut everything forward to end of line    |
| Ctrl + W    | Cut one word backwards using white space as delimiter |
| Ctrl + Y    | Paste whatever was cut by the last cut command |
| Ctrl + H    | Same as backspace                        |
| Ctrl + C    | Kill whatever you are running            |
| Ctrl + D    | Exit the current shell when no process is running, or send EOF to a the running process |
| Ctrl + Z    | Puts whatever you are running into a suspended background process. fg restores it. |
| Ctrl + _    | Undo the last command. (Underscore. So it's actually Ctrl + Shift + minus) |
| Ctrl + T    | Swap the last two characters before the cursor |
| Ctrl + F    | Move cursor one character forward        |
| Ctrl + B    | Move cursor one character backward       |
| Esc + F     | Move cursor one word forward             |
| Esc + B     | Move cursor one word backward            |
| Esc + T     | Swap the last two words before the cursor |
| Tab         | Auto-complete files and folder names     |

### CORE COMMANDS

| Key/Command    | Description                              |
| -------------- | ---------------------------------------- |
| cd             | Home directory                           |
| cd [folder]    | Change directory e.g. `cd documents`     |
| cd /           | Root of drive                            |
| cd -           | Previous directory                       |
| ls             | Short listing                            |
| ls -l          | Long listing                             |
| ls -a          | Listing incl. hidden files               |
| ls -lh         | Long listing with Human readable file sizes |
| ls -R          | Entire content of folder recursively     |
| sudo [command] | Run command with the security privileges of the superuser (Super User DO) |
| open [file]    | Opens a file ( as if you double clicked it ) |
| top            | Displays active processes. Press q to quit |
| nano [file]    | Opens the file using the nano editor     |
| vim [file]     | Opens the file using the vim editor      |
| clear          | Clear screen                             |
| reset          | Resets the terminal display              |

### CHAINING COMMANDS

| Key/Command                | Description                              |
| -------------------------- | ---------------------------------------- |
| [command-a]; [command-b]   | Run command A and then B, regardless of success of A |
| [command-a] && [command-b] | Run command B if A succeeded             |
| [command-a]                |                                          |
| [command-a] &              | Run command A in background              |

### PIPING COMMANDS

| Key/Command                | Description                              |
| -------------------------- | ---------------------------------------- |
| [command-a] \| [command-b] | Run command A and then pass the result to command B e.g ps auxwww \| grep google |

### COMMAND HISTORY

| Key/Command | Description                              |
| ----------- | ---------------------------------------- |
| history n   | Shows the stuff typed – add a number to limit the last n items |
| Ctrl + r    | Interactively search through previously typed commands |
| ![value]    | Execute the last command typed that starts with ‘value’ |
| !!          | Execute the last command typed           |

### FILE MANAGEMENT

| Key/Command              | Description                              |
| ------------------------ | ---------------------------------------- |
| touch [file]             | Create new file                          |
| pwd                      | Full path to working directory           |
| .                        | Current folder, e.g. `ls .`              |
| ..                       | Parent/enclosing directory, e.g. `ls ..` |
| `ls -l ..`               | Long listing of parent directory         |
| `cd ../../`              | Move 2 levels up                         |
| cat                      | Concatenate to screen                    |
| rm [file]                | Remove a file, e.g. `rm data.tmp`        |
| rm -i [file]             | Remove with confirmation                 |
| rm -r [dir]              | Remove a directory and contents          |
| rm -f [file]             | Force removal without confirmation       |
| cp [file] [newfile]      | Copy file to file                        |
| cp [file] [dir]          | Copy file to directory                   |
| mv [file] [new filename] | Move/Rename, e.g. `mv file1.ad /tmp`     |
| pbcopy < [file]          | Copies file contents to clipboard        |
| pbpaste                  | Paste clipboard contents                 |
| pbpaste > [file]         | Past clipboard contents into file, `pbpaste > paste-test.txt` |

### DIRECTORY MANAGEMENT

| Key/Command            | Description                              |
| ---------------------- | ---------------------------------------- |
| mkdir [dir]            | Create new directory                     |
| mkdir -p [dir]/[dir]   | Create nested directories                |
| rmdir [dir]            | Remove directory ( only operates on empty directories ) |
| rm -R [dir]            | Remove directory and contents            |
| [command] \| [command] | Allows to combine multiple commands that generate output, e.g. `cat data.txt |
| less                   | Output content delivered in screensize chunks |
| [command] > [file]     | Push output to file, keep in mind it will get overwritten |
| [command] >> [file]    | Append output to existing file           |
| [command] < [file]     | Tell command to read content from a file |

### SEARCH

| Key/Command                       | Description                              |
| --------------------------------- | ---------------------------------------- |
| find [dir] -name [search_pattern] | Search for files, e.g. `find /Users -name "file.txt"` |
| grep [search_pattern] [file]      | Search for all lines that contain the pattern, e.g. `grep "Tom" file.txt` |
| grep -r [search_pattern] [file]   | Recursively search for all lines that do not contain the pattern |
| grep -v [search_pattern] [file]   | Search for all lines that do NOT contain the pattern |

### HELP

| Key/Command              | Description                              |
| ------------------------ | ---------------------------------------- |
| [command] -h             | Offers help                              |
| [command] —help          | Offers help                              |
| info [command]           | Offers help                              |
| man [command]            | Show the help manual for [command]       |
| whatis [command]         | Gives a one-line description of [command] |
| apropos [search-pattern] | Searches for command with keywords in description |

