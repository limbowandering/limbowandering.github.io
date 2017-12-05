---
title: python多进程多线程通信
date: 2017-10-09 14:00:00
categories: Python
tags: [Python]
---

## 多进程通信

写的一个小作业. 感觉有意思也有必要稍微记下来. 多进程和多线程还是很重要的.

``` python
from multiprocessing import Process, Queue
import os,time,random
import sys

#写进程要做的操作
def write(q):
    print('Process to write: %s' % os.getpid())

    #Ubuntu,Mac等 下面默认子进程不打开标准输入,读 什么的.. 写上这个就行了
    sys.stdin = os.fdopen(0)

    while True:
        t = input('Enter something:')
        #终止条件
        if t == 'q':
            break

        q.put(t)
        time.sleep(random.random())

#读进程做的操作
def read(q):
    print('Process to read: %s' % os.getpid())

    while True:
        value = q.get(True)
        if value == 'q':
            break

        print('Received: %s' % value)

if __name__ == '__main__':
	#父进程创建Queue,并传给各个子进程
    q = Queue()
    pw = Process(target=write, args=(q,))
    pr = Process(target=read, args=(q,))
    #启动子进程pw
    pw.start()
    pr.start()
    #等待pw结束
    pw.join()
    #pr进程里是死循环, 强行终止
    pr.terminate()

```

## 多线程通信

``` python
import os, threading, time, random, sys
from multiprocessing import Queue

#写线程
def Write(q):

    sys.stdin = os.fdopen(0)

    while True:
        t = input('Enter something:')
        q.put(t)  
        time.sleep(random.random())

        if t == 'q':
            break
#读线程
def Read(q):
    while True:
        value = q.get(True)
        print('Received: %s' % value)
        if value == 'q':
            break

if __name__ == '__main__':
    q = Queue()
    tw = threading.Thread(target=Write, args=(q,))
    tr = threading.Thread(target=Read, args=(q,))
    tw.start()
    tr.start()
    
```

