---
title: Python实现socket通信
date: 2017-10-16
categories: Python
tags: [Python]
---

上次的Python多进程多线程通信, 有些简略了,而且有主次进程的关系, 这次用socket通信就是两个进程之间的通信了.

参考文章: [Socket编程详解](https://gist.github.com/kevinkindom/108ffd675cb9253f8f71#socket-函数) [廖雪峰python](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001432004374523e495f640612f4b08975398796939ec3c000#0)

## Python Socket编程

Socket是网络编程的一个抽象概念。通常我们用一个Socket表示“打开了一个网络链接”，而打开一个Socket需要知道目标计算机的IP地址和端口号，再指定协议类型即可。

创建`Socket`时，`AF_INET`指定使用IPv4协议，如果要用更先进的IPv6，就指定为`AF_INET6`。`SOCK_STREAM`指定使用面向流的TCP协议，这样，一个`Socket`对象就创建成功，但是还没有建立连接。

#### 服务器端Socket函数:

| Socket 函数         | 描述                                       |
| ----------------- | ---------------------------------------- |
| s.bind(address)   | 将套接字绑定到地址，在AF_INET下，以tuple(host, port)的方式传入，如s.bind((host, port)) |
| s.listen(backlog) | 开始监听TCP传入连接，backlog指定在拒绝链接前，操作系统可以挂起的最大连接数，该值最少为1，大部分应用程序设为5就够用了 |
| s.accept()        | 接受TCP链接并返回（conn, address），其中conn是新的套接字对象，可以用来接收和发送数据，address是链接客户端的地址。 |

#### 客户端Socket函数

| Socket 函数             | 描述                                       |
| --------------------- | ---------------------------------------- |
| s.connect(address)    | 链接到address处的套接字，一般address的格式为tuple(host, port)，如果链接出错，则返回socket.error错误 |
| s.connect_ex(address) | 功能与s.connect(address)相同，但成功返回0，失败返回errno的值 |

#### 公共Socket函数

**公共 Socket 函数**

| Socket 函数               | 描述                                       |
| ----------------------- | ---------------------------------------- |
| s.recv(bufsize[, flag]) | 接受TCP套接字的数据，数据以字符串形式返回，buffsize指定要接受的最大数据量，flag提供有关消息的其他信息，通常可以忽略 |
| s.send(string[, flag])  | 发送TCP数据，将字符串中的数据发送到链接的套接字，返回值是要发送的字节数量，该数量可能小于string的字节大小 |
| s.close()               | 关闭套接字                                    |

经过了多少次的bug之后: 我给我服务端能跑多线程的 和 客户端的代码:

服务端:

``` python
import socket
import sys
import threading

HOST = '127.0.0.1'
PORT = 8233

try:
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
except socket.error:
    print("Socket created failed")
    sys.exit()

try:
    s.bind((HOST, PORT))
except socket.error:
    print("Bind failed")
    sys.exit()

s.listen(5)

print("Server start at: %s:%s" % (HOST, PORT))
print("Wait for connection")

def tcplink(connection, address):
    print("Accept new connection from %s:%s..." % address)
    connection.send("Welcome".encode())
    while True:
        data = connection.recv(1024)
        if data.decode('utf-8') == 'exit':
            break
        connection.send("Received".encode('utf-8'))
    connection.close()
    print("Connection from %s closed." % connection)

while True:
    connection, address = s.accept()
    t = threading.Thread(target=tcplink, args=(connection, address))
    t.start()

```

客户端:

``` python
import socket

HOST = '127.0.0.1'
PORT = 8233

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((HOST, PORT))

while True:
    msg = input("Please input msg:")

    s.send(msg.encode())
    data = s.recv(1024)

    print(data.decode('utf-8'))
    if msg == "exit":
        s.close()
        break

```

## 更进一步

这个也是作业的问题了, 想开另一个进程控制现有的进程进行拍照或者退出.

最终结果如下:

OpenCV的进程:

``` python
import socket
import sys
import cv2

HOST = '127.0.0.1'
PORT = 8233

try:
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
except socket.error:
    print("Socket created failed")
    sys.exit()

try:
    s.bind((HOST, PORT))
except socket.error:
    print("Bind failed")
    sys.exit()


s.listen(1)

print("Server start at: %s:%s" % (HOST, PORT))
print("Wait for connection")

connection, address = s.accept()

cap = cv2.VideoCapture(0)
ret, frame = cap.read()
gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
cv2.imshow('frame', gray)


print("Accept new connection from %s:%s" % address)
connection.send("Welcome".encode())
while True:
    data = connection.recv(1024)
    if data.decode('utf-8') == 'exit':
        break
    if data.decode('utf-8') == 'c':
        cv2.imwrite('pic2.jpg', frame)
    connection.send("Received".encode('utf-8'))
connection.close()
print("Connection from %s closed." % connection)

```

控制进程:

``` python
import socket

HOST = '127.0.0.1'
PORT = 8233

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((HOST, PORT))

while True:
    msg = input("Please input msg:")

    s.send(msg.encode())
    data = s.recv(1024)

    print(data.decode('utf-8'))
    if msg == "exit":
        s.close()
        break

```

今天感觉解决了不少bug, 很开心.