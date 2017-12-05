---
title: 班级同学最相似体型的计算2
date: 2017-10-30
categories: Python
tags: [Python]
---

上次写的代码实在是太烂了, 而且因为应用场景是自己班级同学, 也没有考虑如果其他同学服务端格式有问题怎么办, 于是上周上课时候我先是半小时把链接错误排掉再半小时发现是有的同学衣服尺码写的X,L啊是小写的, 或者写错的之类的, 本地检查一遍不就好了. 所以应该是先写入文件, 再分析结果. 于是代码重写一下如下.

先继续说一下场景: 在班级机房中, 每个人开一个服务器线程, 当收到客户端发过来的get请求时候, 返回自己的学号, 身高, 体重, 穿衣服尺码. 每个人写一个客户端, (机房IP地址是连续的40个)爬取同学们的信息, 然后本地计算哪两位同学体型最相似.

server.py

``` python
#coding:utf-8
import socket
import sys
import threading
#import json

HOST = '192.168.135.181'
PORT = 2333

try:
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
except socket.error:
    print("socket created failed")
    sys.exit()

try:
    s.bind((HOST,PORT))
except socket.error:
    print("Bind failed")
    sys.exit()

s.listen(31)

print("Server start at: %s:%s" % (HOST,PORT))
print("Wait for connection")

def tcplink(connection,address):
    print("Accept new connection from %s:%s..." % address)

    while True:
        data=connection.recv(1024)
        if data.decode('utf-8')=='exit':
            break
        if data.decode('utf-8')=='get':
            connection.send("1527406002;172;71;L".encode('utf-8'))
        else:
            connection.send("Received".encode('utf-8'))
    connection.close()
    print("connection from %s closed." % connection)


while True:
    connection, address = s.accept()
    t=threading.Thread(target=tcplink, args=(connection,address))
    t.start()

```

同学们只要改掉里头的个人信息就行了.

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

client.py

``` python
# -*- coding:utf-8 -*-
import socket


def write_to_file(content):
    file_handler = open('data.txt', 'a', encoding='utf-8')
    file_handler.write(content+'\n')


def connect_to_server(ip):
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.settimeout(2)
    try:
        s.connect((ip, 2333))
        s.send('get'.encode('utf-8'))
        data = s.recv(1024)
        data = data.decode('utf-8')
        if data != "Received":
            print('收到来自服务器的消息' + data)
            write_to_file(data)
        s.send('exit'.encode('utf-8'))
        s.close()
    except socket.timeout:
        print('%s连接超时' % (ip))


if __name__ == "__main__":
    ip = '192.168.135.'
    i = 181
    while i <= 220:
        ips = ip + str(i)
        connect_to_server(ips)
        i += 1

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



示例一下爬取结果:

data.txt

``` 
1527406002;172;71;L
1527406030;160;46;S
1527406011;165;50;M
1527406015;165;57;L
1527406012;179;85;XL
1527406043;100;50;L
1527406058;195;80;XXXL
1527406045;176;58;L
1527406027;170;75;L
1527406053;175;82;XXL
1527406051;180;68;XL
1527406046;172;50;M
1527406044;171;60;XL
1527406034;178;80;XXXL
1527406010;173;67;M
1527406056;172;60;L
1527406032;180;70;L
1527406048;160;50;M
1527406023;176;58;L
1527406059;172;76;XL
1527406022;159;49;M

```



用client爬取数据, 考虑了超时情况, 然后检查data.txt,检查异常,然后运行analyse






