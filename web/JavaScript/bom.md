---
title: BOM操作
description: 
published: true
date: 2023-03-26T08:05:34.731Z
tags: 
editor: markdown
dateCreated: 2023-02-25T11:34:11.515Z
---

## BOM

BOM（Browser Object Model）即浏览器对象模型，它提供了独立于内容而与浏览器窗口进行交互的对象，其核心对象是 window。

BOM 由一系列相关的对象构成，并且每个对象都提供了很多方法与属性。

BOM 缺乏标准，JavaScript 语法的标准化组织是 ECMA，DOM 的标准化组织是 W3C，BOM 最初是Netscape 浏览器标准的一部分。

**DOM**

* 文档对象模型
* DOM就是把「文档」当做一个「对象」来看待
* DOM的顶级对象是document
* DOM主要学习的是操作页面元素DOM是W3C标准规范

**BOM**

* 浏览器对象模型
* 把「浏览器」当做一个「对象」来看待
* BOM的顶级对象是window
* BOM学习的是浏览器窗口交互的一些对象
* BOM是浏览器厂商在各自浏览器上定义的，兼容性较差

### BOM的构成

> BOM 比 DOM 更大，它包含 DOM。

### 顶级对象window

**window对象是浏览器的顶级对象，它具有双重角色**。 1．它是JS访问浏览器窗口的一个接口。 2．它是一个全局对象。定义在全局作用域中的变量、函数都会变成window对象的属性和方法。

在调用的时候可以省略window，前面学习的对话框都属于window对象方法，如alert()、prompt()等。 注意: [window下的一个特殊属性window.name](http://xn--windowwindow-lt4sze8su09pxpndl2cts1am5q.name)

### window对象的常见事件
- [窗口加载](/JavaScript/bom/窗口加载)
- [窗口大小](/JavaScript/bom/窗口大小)
- [定时器](/JavaScript/bom/定时器)
- [location](/JavaScript/bom/location)
- [navigator](/JavaScript/bom/navigator)
- [history](/JavaScript/bom/history)
- [元素偏移](/JavaScript/bom/元素偏移)
- [元素滚动](/JavaScript/bom/元素滚动)
- [元素可视区](/JavaScript/bom/元素可视区)

