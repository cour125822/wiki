---
title: udp通讯
description: 
published: true
date: 2023-03-26T08:05:20.682Z
tags: 
editor: markdown
dateCreated: 2023-02-26T07:59:59.526Z
---

# UDP套接字

### 服务端

`import socket server = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)  # 数据报协议-》UDP server.bind(('127.0.0.1', 8080)) while True:     data, client_addr = server.recvfrom(1024)     print('===>', data, client_addr)     server.sendto(data.upper(), client_addr) server.close()`

### 客户端

`import socket client = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)  # 数据报协议-》UDP while True:     msg = input('>>: ').strip()  # msg=''     client.sendto(msg.encode('utf-8'), ('127.0.0.1', 8080))     data, server_addr = client.recvfrom(1024)     print(data) client.close()`

* UDP是无链接的，先启动哪一端都不会报错
* UDP协议是数据报协议，发空的时候也会自带报头，因此客户端输入空，服务端也能收到

## UPD套接字无粘包问题

### 3.1 服务端

`import socket server = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)  # 数据报协议-》udp server.bind(('127.0.0.1', 8080)) data, client_addr = server.recvfrom(1024)  # b'hello'==>b'h' print('第一次：', client_addr, data) data, client_addr = server.recvfrom(1024)  # b'world' =>b'world' print('第二次：', client_addr, data)

# 

# data,client_addr=server.recvfrom(1024)

# print('第三次：',client_addr,data)

server.close()`

### 客户端

`import socket client = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)  # 数据报协议-》udp client.sendto('hello'.encode('utf-8'), ('127.0.0.1', 8080)) client.sendto('world'.encode('utf-8'), ('127.0.0.1', 8080))

# client.sendto(''.encode('utf-8'),('127.0.0.1',8080))

client.close()`

* UPD协议一般不用于传输大数据。
* UDP套接字虽然没有粘包问题，但是不能替代TCP套接字，因为UPD协议有一个缺陷：如果数据发送的途中，数据丢失，则数据就丢失了，而TCP协议则不会有这种缺陷，因此一般UPD套接字用户无关紧要的数据发送，例如qq聊天。

## qq聊天

* 由于UDP无连接，所以可以同时多个客户端去跟服务端通信

### 服务端

`#_*_coding:utf-8_*_ __author__ = 'lqz' import socket ip_port = ('127.0.0.1', 8081) UDP_server_sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)  #买手机 UDP_server_sock.bind(ip_port) while True:     qq_msg, addr = UDP_server_sock.recvfrom(1024)     print('来自[%s:%s]的一条消息:\\033[1;44m%s\\033[0m' %           (addr[0], addr[1], qq_msg.decode('utf-8')))     back_msg = input('回复消息: ').strip()     UDP_server_sock.sendto(back_msg.encode('utf-8'), addr)`

### 客户端

`#_*_coding:utf-8_*_ __author__ = 'lqz' import socket BUFSIZE = 1024 UDP_client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM) qq_name_dic = {     '狗哥alex': ('127.0.0.1', 8081),     '瞎驴': ('127.0.0.1', 8081),     '一棵树': ('127.0.0.1', 8081),     '武大郎': ('127.0.0.1', 8081), } while True:     qq_name = input('请选择聊天对象: ').strip()     while True:         msg = input('请输入消息,回车发送: ').strip()         if msg == 'quit': break         if not msg or not qq_name or qq_name not in qq_name_dic: continue         UDP_client_socket.sendto(msg.encode('utf-8'), qq_name_dic[qq_name])         back_msg, addr = UDP_client_socket.recvfrom(BUFSIZE)         print('来自[%s:%s]的一条消息:\\033[1;44m%s\\033[0m' %               (addr[0], addr[1], back_msg.decode('utf-8'))) UDP_client_socket.close()`