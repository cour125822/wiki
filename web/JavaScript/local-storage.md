---
title: 本地存储
description: 
published: true
date: 2023-03-26T08:05:49.452Z
tags: 
editor: markdown
dateCreated: 2023-02-25T11:33:21.388Z
---

## 

随着互联网的快速发展，基于网页的应用越来越普遍，同时也变的越来越复杂，为了满足各种各样的需求，会经常性在本地存储大量的数据，HTML5规范提出了相关解决方案。

### 本地存储特性

1、数据存储在用户浏览器中

2、设置、读取方便、甚至页面刷新不丢失数据

3、容量较大，sessionStorage约5M、localStorage约20M

4、只能存储字符串，可以将对象JSON.stringify() 编码后存储

### window.sessionStorage

1、生命周期为关闭浏览器窗口

2、在同一个窗口(页面)下数据可以共享

3、以键值对的形式存储使用

存储数据：

`sessionStorage.setItem(key, value)`

获取数据：

`sessionStorage.getItem(key)`

删除数据：

`sessionStorage.removeItem(key)`

清空数据：(所有都清除掉)

`sessionStorage.clear()`

### window.localStorage

1、声明周期永久生效，除非手动删除 否则关闭页面也会存在

2、可以多窗口（页面）共享（同一浏览器可以共享）

3. 以键值对的形式存储使用

存储数据：

`localStorage.setItem(key, value)`

获取数据：

`localStorage.getItem(key)`

删除数据：

`localStorage.removeItem(key)`

清空数据：(所有都清除掉)

`localStorage.clear()`