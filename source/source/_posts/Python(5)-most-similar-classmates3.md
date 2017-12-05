---
title: 班级同学最相似体型的计算3
date: 2017-10-31
categories: Python
tags: [Python,Flask]
---

这是第二次的作业, 把服务端改成web API的形式.  

场景依然简述如下: 在班级机房中, 每个人开一个服务器线程, 当收到客户端发过来的get请求时候, 返回自己的学号, 身高, 体重, 穿衣服尺码. 每个人写一个客户端, (机房IP地址是连续的40个)爬取同学们的信息, 然后本地计算哪两位同学体型最相似.

## 思路与说明

假设机房电脑没有python环境, 那么需要一步一步这么安装几个东西:

- python3.X(官网找) 并加入环境变量

- sublime: (编辑器还是要有的吧...)

  - 打开配置文件, 添加以下:

    ``` 
    ,
    "translate_tabs_to_spaces": true
    //注释: 最后一条不要加逗号
    //这是保证格式统一, 把TAB转化成空格的, 可以选中代码看一下
    ```

- ` pip install virtualenv` 安装virtualenv

- 切换到代码目录后` virtualenv venv` 创建一个工作环境

  注意, 因为假定机房只有python3.5 如果有多个python, 需要指定python的版本` virtualenv -p python3 venv` 

- `pip install flask` 写web API我是用的flask

- `pip install numpy` 后来的计算用到了numpy 如果不用到的这个可以不用.

Windows下用` venv\scripts\activate`激活环境,用`venv\scripts\deactivate` 退出环境.

进入环境后, `python server.py` 就可以了, 下面给出代码, 注释一些需要改动的地方.

这里先写一个简单实现, 考完期中考试再改的好一点, 数据格式用json, 客户端有GUI的.

## 代码

server是服务端, client去获取信息,存储到data.txt 然后看看, 然后用analyse去计算结果.

server.py

``` python
#coding:utf-8
from flask import Flask
app = Flask(__name__)

@app.route('/')
def my_info():
    return '1527406002;172;72;L'

if __name__ == '__main__':
    app.run(host='10.40.45.55',port=2333,debug=True)
    
#app.run(host='10.40.45.55',port=2333,debug=True,threaded=True)
#其实对于简单的多线程, flask已经提供了并发支持, 这里threaded=True我并没有测试, 明天试试吧.
#把host改成机房192什么的, port我们还是用2333,debug=True能看到谁访问了你
```

client.py

``` python
#coding:utf-8
import urllib.request
import socket

def write_to_file(content):
    file_handler = open('data.txt','a',encoding='utf-8')
    file_handler.write(content+'\n')

def connect_to_server(ip):
    response = urllib.request.urlopen(ip)
    string = response.read()
    string = bytes(string).decode('utf-8')
    write_to_file(string)

if __name__ == '__main__':
    ip = 'http://10.40.45.55:2333'
    socket.setdefaulttimeout(4)
    connect_to_server(ip)

#这只是本地测试的代码,主函数需要把ip循环40个去获取, 这个自己改吧. setdefaulttimeout 4s, 这个我发现有时候会超时, 原因还没搞懂.
```

analyse.py

``` python
# -*- coding:utf-8 -*-
from Utils import read_from_file, compare
from Student import Student


if __name__ == "__main__":
    lines = read_from_file()
    line_list = []
    for i in lines:
        component = i.split(';')
        one = Student(component[0], component[1], component[2], component[3])
        line_list.append(one)
    res = compare(line_list)
    people = [line_list[res[0]].number, line_list[res[1]].number]
    print(people)
```

Student.py

``` python
# -*- coding:utf-8 -*-


class Student(object):
    def __init__(self, number, height, weight, size):
        self.number = number
        self.height = height
        self.weight = weight
        self.size = size

    def __str__(self) -> str:
        return 'number:' + self.number + ',height:' + self.height + ',weight:' + self.weight + ',size:' + self.size

```

Utils.py

``` python
# -*- coding:utf-8 -*-
import numpy


def read_from_file():
    f = open('data.txt', encoding='utf-8')
    lines = []
    line = f.readline()
    while line:
        lines.append(line)
        line = f.readline()
    return lines


def calEuclideanDistance(p1, p2):
    return numpy.sqrt(numpy.square(float(p1.height) - float(p2.height))+numpy.square(float(p1.weight) - float(p2.weight)))


def compare(people):
    i = 0
    similar = 999
    mark = [None,None]
    while i < len(people)-1:
        j = i+1
        person = people[i]
        while j < len(people):
            euclideanDistance = calEuclideanDistance(person, people[j])
            if similar > euclideanDistance:
                similar = euclideanDistance
                mark[0] = i
                mark[1] = j
            j += 1
        i += 1
    print(similar)
    return mark

```

大概本地能看到这样的数据吧, 这是上次了:

``` 
1527406002;172;71;L
1527406030;XXX;XX;X
1527406011;XXX;XX;X
1527406015;XXX;XX;X
1527406012;XXX;XX;X
1527406043;XXX;XX;X
1527406058;XXX;XX;X
1527406045;XXX;XX;X
1527406027;XXX;XX;X
1527406053;XXX;XX;X
1527406051;XXX;XX;X
1527406046;XXX;XX;X
1527406044;XXX;XX;X
1527406034;XXX;XX;X
1527406010;XXX;XX;X
1527406056;XXX;XX;X
1527406032;XXX;XX;X
1527406048;XXX;XX;X
1527406023;XXX;XX;X
1527406059;XXX;XX;X
1527406022;XXX;XX;X

```

不能透漏个人信息.





