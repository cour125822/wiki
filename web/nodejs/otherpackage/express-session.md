---
title: express-session
description: 
published: true
date: 2023-03-26T08:10:07.713Z
tags: 
editor: markdown
dateCreated: 2023-02-26T02:25:34.476Z
---

### express-session

cookie：浏览器在电脑硬盘中开辟的一块空间，主要供服务器端存储数据。

1. cookie中的数据是以域名的形式进行区分的。
2. cookie中的数据是有过期时间的，超过时间数据会被浏览器自动删除。
3. cookie中的数据会随着请求被自动发送到服务器端。

session：实际上就是一个对象，存储在服务器端的内存中，在session对象中也可以存储多条数据，每一条数据都有一个sessionid做为唯一标识。

![https://raw.githubusercontent.com/cloutp/blog_img/main/202201281206243.png](https://raw.githubusercontent.com/cloutp/blog_img/main/202201281206243.png)

`const session = require('express-session'); app.use(session({ secret: 'secret key' }));`