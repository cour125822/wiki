---
title: Web Workers
description: 
published: true
date: 2023-03-06T03:14:17.816Z
tags: 
editor: markdown
dateCreated: 2023-02-25T11:35:39.351Z
---

## **H5 Web Workers**

* 可以让js在分线程执行
* Worker

  ```
  var worker = new Worker('worker.js');
  worker.onMessage = function(event){event.data} : 用来接收另一个线程发送过来的数据的回调
  worker.postMessage(data1) : 向另一个线程发送数据
  ```
* 问题:

  * worker内代码不能操作DOM更新UI
  * 不是每个浏览器都支持这个新特性
  * 不能跨域加载JS
* svn版本控制
* svn server