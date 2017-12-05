---
title: 班级同学最相似体型的计算
date: 2017-10-24
categories: Python
tags: [Python, json]
---

场景: 在班级机房中, 每个人开一个服务器线程, 当收到客户端发过来的get请求时候, 返回自己的学号, 身高, 体重, 穿衣服尺码.  每个人写一个客户端, (机房IP地址是连续的40个)爬取同学们的信息, 然后本地计算哪两位同学体型最相似.

哈! 明天上课再来实际测试, 今天先写一个beta版. 主要思路: 用json传递文件. 服务端开多线程支持30个同学访问. 用字典表示一个同学的信息. 客户端进行计划后用图形化界面显示最终结果.

![](../../../../images/mine/SimilarClassmates.png)

以下为代码:

Server.py

``` python
#coding:utf-8
import socket
import sys
import threading
import json

HOST = '127.0.0.1'
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
            connection.send(json.dumps({'id': 1527406002,
                                        'height': 172,
                                        'weight': 71,
                                        'size': 'L'}).encode('utf-8'))
        else:
            connection.send("Received".encode('utf-8'))
    connection.close()
    print("connection from %s closed." % connection)


while True:
    connection, address = s.accept()
    t=threading.Thread(target=tcplink, args=(connection,address))
    t.start()
```

client.py

``` python 
#coding:utf-8
import socket
import json
import tkinter as tk

def client_recv(stu):
    while True:
        msg = input("Enter sth. ")

        if msg == 'exit':
            break

        s.send(msg.encode('utf-8'))
        data = s.recv(1024)
        data = json.loads(data.decode('utf-8'))

        if msg == 'get':
            stu.append(data)

        print(data)

def trans_size(s):
    if s['size'] == 'S':
        s['size'] = 0
    if s['size'] == 'M':
        s['size'] = 1
    if s['size'] == 'L':
        s['size'] = 2
    if s['size'] == 'XL':
        s['size'] = 3
    if s['size'] == 'XXL':
        s['size'] = 4
    if s['size'] == 'XXXL':
        s['size'] = 5

def compute(result,stu):
    length = len(stu)
    for s in stu:
        trans_size(s)

    i=0
    while i<length :
        j=i+1
        while j<length:
            t = abs(stu[i]['height']-stu[j]['height']) +abs(stu[i]['weight']-stu[j]['weight']) +abs(stu[i]['size']-stu[j]['size'])
            result.append({'id1': stu[i]['id'],
                            'id2': stu[j]['id'],
                            'result': t})
            j = j+1
        i = i+1
    tmp = result[0]
    for s in result:
        if s['result'] < tmp['result']:
            tmp = s

    id_1 = str(tmp['id1'])
    id_2 = str(tmp['id2'])
    res = 'Gay1:' + id_1 + ' Gay2: ' + id_2
    return res

def show_window(res):
    window = tk.Tk()
    window.title('在一起')
    window.geometry('600x400')

    var = tk.StringVar()
    l = tk.Label(window, textvariable=var, bg='green', font=('Arial', 12), width=60, height=5)
    l.pack()

    on_hit = False
    def hit_me():
        var.set(res)

    b = tk.Button(window, text='Who are the couple!', width=35,
                  height=2, command=hit_me)
    b.pack()

    window.mainloop()

if __name__ == '__main__':

    HOST = '127.0.0.1'
    PORT = 2333

    stu_b = {
        'id': 1527406003,
        'height': 174,
        'weight': 73,
        'size': 'XL'
    };

    stu_c = {
        'id': 1527406007,
        'height': 140,
        'weight': 51,
        'size': 'M'
    };

    stu = [stu_b, stu_c];
    result = list()

    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect((HOST,PORT))



    client_recv(stu)
    final = compute(result,stu)
    show_window(final)


```

啊, 代码写的还是太少了, 感觉写的很丑, 要多练习啊!