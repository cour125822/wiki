---
title: Jquery Ajax
description: 
published: true
date: 2023-03-06T03:01:59.276Z
tags: 
editor: markdown
dateCreated: 2023-02-26T01:50:31.858Z
---

## 

### 发送Ajax请求

`$.ajax({      type: 'get',      url: '<http://www.example.com>',      data: { name: 'zhangsan', age: '20' },      contentType: 'application/x-www-form-urlencoded',      // 在请求发送之前调用      beforeSend: function () {         alert('请求不会被发送')         // 请求不会被发送         return false;      },      success: function (response) {},      error: function (xhr) {} });`

`$.ajax({      type: 'get',      url: '<http://www.example.com>',      data: JSON.stringify({name: 'zhangsan', age: '20'}),      contentType: 'application/json',      beforeSend: function () {           return false      },      success: function (response) {},      error: function (xhr) {} });`

### 发送jsonp请求

`$.ajax({     url: '<http://www.example.com>',     // 指定当前发送jsonp请求     dataType: 'jsonp',     // 修改callback参数名称     jsonp: 'cb',     // 指定函数名称     jsonCallback: 'fnName',     success: function (response) {}  })`

### serialize方法

> 将表单中的数据自动拼接成字符串类型的参数

`var params = $('#form').serialize(); // name=zhangsan&age=30`

### 、、.get()、.post()方法概述

> 方法用于发送请求，方法用于发送请求，.get方法用于发送get请求，.post方法用于发送post请求

`$.get('<http://www.example.com>', {name: 'zhangsan', age: 30}, function (response) {}) $.post('<http://www.example.com>', {name: 'lisi', age: 22}, function (response) {})`

### 全局事件

> 只要页面中有Ajax请求被发送，对应的全局事件就会被触发

`.ajaxStart()     // 当请求开始发送时触发 .ajaxComplete()  // 当请求完成时触发`

### NProgress

> 官宣：纳米级进度条，使用逼真的涓流动画来告诉用户正在发生的事情！

`<link rel='stylesheet' href='nprogress.css'/> <script src='nprogress.js'></script>`

`NProgress.start();  // 进度条开始运动  NProgress.done();   // 进度条结束运动`