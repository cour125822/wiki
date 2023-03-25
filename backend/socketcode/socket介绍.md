---
title: socket介绍
description: 
published: true
date: 2023-03-06T05:53:02.720Z
tags: 
editor: markdown
dateCreated: 2023-02-26T07:54:44.591Z
---

# socket介绍

# 

## Scoket

Socket是应用层与TCP/IP协议族通信的中间软件抽象层，它是一组接口。在设计模式中，Socket其实就是一个门面模式，它把复杂的TCP/IP协议族隐藏在Socket接口后面，对用户来说，一组简单的接口就是全部，让Socket去组织数据，以符合指定的协议。

所以，我们无需深入理解tcp/udp协议，socket已经为我们封装好了，我们只需要遵循socket的规定去编程，写出的程序自然就是遵循tcp/udp标准的。

* 注意：也有人将socket说成ip+port，ip是用来标识互联网中的一台主机的位置，而port是用来标识这台机器上的一个应用程序，ip地址是配置到网卡上的，而port是应用程序开启的，ip与port的绑定就标识了互联网中独一无二的一个应用程序，而程序的pid是同一台机器上不同进程或者线程的标识。

## 套接字发展史及分类

套接字起源于 20 世纪 70 年代加利福尼亚大学伯克利分校版本的 Unix,即人们所说的 BSD Unix。 因此,有时人们也把套接字称为“伯克利套接字”或“BSD 套接字”。一开始,套接字被设计用在同 一台主机上多个应用程序之间的通讯。这也被称进程间通讯,或 IPC。套接字有两种（或者称为有两个种族）,分别是基于文件型的和基于网络型的。

### 基于文件类型的套接字家族

套接字家族的名字：AF_UNIX

unix一切皆文件，基于文件的套接字调用的就是底层的文件系统来取数据，两个套接字进程运行在同一机器，可以通过访问同一个文件系统间接完成通信

### 基于网络类型的套接字家族

套接字家族的名字：AF_INET

(还有AF_INET6被用于ipv6，还有一些其他的地址家族，不过，他们要么是只用于某个平台，要么就是已经被废弃，或者是很少被使用，或者是根本没有实现，所有地址家族中，AF_INET是使用最广泛的一个，python支持很多种地址家族，但是由于我们只关心网络编程，所以大部分时候我么只使用AF_INET)

## 套接字工作流程

一个生活中的场景。你要打电话给一个朋友，先拨号，朋友听到电话铃声后提起电话，这时你和你的朋友就建立起了连接，就可以讲话了。等交流结束，挂断电话结束此次交谈。 生活中的场景就解释了这工作原理。

![https://cloutp.github.io/wiki/Python/08网络编程/02Socket操作/images/007S8ZIlly1gjrnjqumlcj30da0dnwex.jpg](https://cloutp.github.io/wiki/Python/08%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B/02Socket%E6%93%8D%E4%BD%9C/images/007S8ZIlly1gjrnjqumlcj30da0dnwex.jpg)

先从服务器端说起。服务器端先初始化Socket，然后与端口绑定(bind)，对端口进行监听(listen)，调用accept阻塞，等待客户端连接。在这时如果有个客户端初始化一个Socket，然后连接服务器(connect)，如果连接成功，这时客户端与服务器端的连接就建立了。客户端发送数据请求，服务器端接收请求并处理请求，然后把回应数据发送给客户端，客户端读取数据，最后关闭连接，一次交互结束，使用以下Python代码实现：

`import socket

# socket_family 可以是 AF_UNIX 或 AF_INET。socket_type 可以是 SOCK_STREAM 或 SOCK_DGRAM。protocol 一般不填，默认值为 0

socket.socket(socket_family, socket_type, protocal=0)

# 获取tcp/ip套接字

tcpSock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# 获取udp/ip套接字

udpSock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

# 由于 socket 模块中有太多的属性。我们在这里破例使用了'from module import *'语句。使用 'from socket import *'，我们就把 socket 模块里的所有属性都带到我们的命名空间里了，这样能大幅减短我们的代码

tcpSock = socket(AF_INET, SOCK_STREAM)`

### 服务端套接字函数

| 方法       | 用途                                         |
| ------------ | ---------------------------------------------- |
| s.bind()   | 绑定(主机,端口号)到套接字                    |
| s.listen() | 开始TCP监听                                  |
| s.accept() | 被动接受TCP客户的连接,(阻塞式)等待连接的到来 |

### 客户端套接字函数

| 方法           | 用途                                                    |
| ---------------- | --------------------------------------------------------- |
| s.connect()    | 主动初始化TCP服务器连接                                 |
| s.connect_ex() | connect()函数的扩展版本,出错时返回出错码,而不是抛出异常 |

### 公共用途的套接字函数

| 方法            | 用途                                                                                                                  |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------- |
| s.recv()        | 接收TCP数据                                                                                                           |
| s.send()        | 发送TCP数据(send在待发送数据量大于己端缓存区剩余空间时,数据丢失,不会发完)                                             |
| s.sendall()     | 发送完整的TCP数据(本质就是循环调用send,sendall在待发送数据量大于己端缓存区剩余空间时,数据不丢失,循环调用send直到发完) |
| s.recvfrom()    | 接收UDP数据                                                                                                           |
| s.sendto()      | 发送UDP数据                                                                                                           |
| s.getpeername() | 连接到当前套接字的远端的地址                                                                                          |
| s.getsockname() | 当前套接字的地址                                                                                                      |
| s.getsockopt()  | 返回指定套接字的参数                                                                                                  |
| s.setsockopt()  | 设置指定套接字的参数                                                                                                  |
| s.close()       | 关闭套接字                                                                                                            |

### 面向锁的套接字方法

| 方法            | 用途                         |
| ----------------- | ------------------------------ |
| s.setblocking() | 设置套接字的阻塞与非阻塞模式 |
| s.settimeout()  | 设置阻塞套接字操作的超时时间 |
| s.gettimeout()  | 得到阻塞套接字操作的超时时间 |

### 面向文件的套接字的函数