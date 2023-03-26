---
title: Navigator
description: 
published: true
date: 2023-03-26T08:09:07.335Z
tags: 
editor: markdown
dateCreated: 2023-02-26T01:32:23.934Z
---

### navigator对象

navigator 对象包含有关浏览器的信息，它有很多属性，我们最常用的是 userAgent，该属性可以返回由客户机发送服务器的 user-agent 头部的值。

下面前端代码可以判断用户那个终端打开页面，实现跳转

`if((navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i))) {     window.location.href = "";     //手机  } else {     window.location.href = "";     //电脑  }`