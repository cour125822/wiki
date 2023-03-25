---
title: tcp通讯
description: 
published: true
date: 2023-03-06T05:53:17.081Z
tags: 
editor: markdown
dateCreated: 2023-02-26T07:57:29.927Z
---

# TCP协议通讯

# TCP协议通讯

## 基于TCP协议的套接字编程(简单)

* 可以通过`netstat -an | findstr 8080`查看套接字状态

### 服务端

`import socket #1、买手机 phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  #tcp称为流式协议,udp称为数据报协议SOCK_DGRAM

# print(phone)

#2、插入/绑定手机卡

# phone.setsockopt(socket.SOL_SOCKET,socket.SO_REUSEADDR,1)

phone.bind(('127.0.0.1', 8081)) #3、开机 phone.listen(5)  # 半连接池，限制的是请求数 #4、等待电话连接 print('start....') conn, client_addr = phone.accept()  #（三次握手建立的双向连接，（客户端的ip，端口）） print(conn) print(client_addr) #5、通信：收\发消息 data = conn.recv(1024)  #最大接收的字节数 print('来自客户端的数据', data) conn.send(data.upper())

# import time

# time.sleep(500)

#6、挂掉电话连接 conn.close() #7、关机 phone.close()`

### 客户端

`import socket

# [socket.AF](http://socket.AF)

#1、买手机 phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM) print(phone) #2、拨电话 phone.connect(('127.0.0.1', 8081))  # 指定服务端ip和端口 #3、通信：发\收消息 phone.send('hello'.encode('utf-8'))

# phone.send(bytes('hello',encoding='utf-8'))

data = phone.recv(1024) print(data)

# import time

# time.sleep(500)

#4、关闭 phone.close()`

## 基于TCP协议的套接字编程(循环)

### 服务端

`import socket #1、买手机 phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  #tcp称为流式协议,udp称为数据报协议SOCK_DGRAM

# print(phone)

#2、插入/绑定手机卡

# phone.setsockopt(socket.SOL_SOCKET,socket.SO_REUSEADDR,1)

phone.bind(('127.0.0.1', 8080)) #3、开机 phone.listen(5)  # 半连接池，限制的是请求数 #4、等待电话连接 print('start....') while True:  # 连接循环 conn, client_addr = phone.accept()  #（三次握手建立的双向连接，（客户端的ip，端口）） # print(conn) print('已经有一个连接建立成功', client_addr) #5、通信：收\发消息 while True:  # 通信循环 try: print('服务端正在收数据...') data = conn.recv(1024)  #最大接收的字节数，没有数据会在原地一直等待收，即发送者发送的数据量必须>0bytes # print('===>') if len(data) == 0: break  #在客户端单方面断开连接，服务端才会出现收空数据的情况 print('来自客户端的数据', data) conn.send(data.upper()) except ConnectionResetError: break #6、挂掉电话连接 conn.close() #7、关机 phone.close()`

### 客户端

`import socket #1、买手机 phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# print(phone)

#2、拨电话 phone.connect(('127.0.0.1', 8080))  # 指定服务端ip和端口 #3、通信：发\收消息 while True:  # 通信循环 msg = input('>>: ').strip()  #msg='' if len(msg) == 0: continue phone.send(msg.encode('utf-8')) # print('has send----->') data = phone.recv(1024) # print('has recv----->') print(data) #4、关闭 phone.close()`

### 客户端

`import socket #1、买手机 phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# print(phone)

#2、拨电话 phone.connect(('127.0.0.1', 8080))  # 指定服务端ip和端口 #3、通信：发\收消息 while True:  # 通信循环 msg = input('>>: ').strip() phone.send(msg.encode('utf-8')) data = phone.recv(1024) print(data) #4、关闭 phone.close()`