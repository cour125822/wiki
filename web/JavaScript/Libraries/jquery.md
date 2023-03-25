---
title: Jquery
description: 
published: true
date: 2023-03-06T04:20:01.185Z
tags: 
editor: markdown
dateCreated: 2023-02-26T02:31:08.811Z
---

## jQuery 介绍

**jQuery概况 :**

* jQuery 是一个快速、简洁的 JavaScript 库，其设计的宗旨是“write Less，Do More”，即倡导写更少的代码，做更多的事情。
* j 就是 JavaScript； Query 查询； 意思就是查询js，把js中的DOM操作做了封装，我们可以快速的查询使用里面的功能。
* jQuery 封装了 JavaScript 常用的功能代码，优化了 DOM 操作、事件处理、动画设计和 Ajax 交互。
* 学习jQuery本质： 就是学习调用这些函数（方法）。
* jQuery 出现的目的是加快前端人员的开发速度，我们可以非常方便的调用和使用它，从而提高开发效率。

### jQuery的优点

1. 轻量级。核心文件才几十kb，不会影响页面加载速度。
2. 跨浏览器兼容，基本兼容了现在主流的浏览器。
3. 链式编程、隐式迭代。
4. 对事件、样式、动画支持，大大简化了DOM操作。
5. 支持插件扩展开发。有着丰富的第三方的插件，例如：树形菜单、日期控件、轮播图等。
6. 免费、开源。

### jQuery 的下载

jQuery的官网地址： [https://jquery.com/，官网即可下载最新版本。](https://jquery.com/%EF%BC%8C%E5%AE%98%E7%BD%91%E5%8D%B3%E5%8F%AF%E4%B8%8B%E8%BD%BD%E6%9C%80%E6%96%B0%E7%89%88%E6%9C%AC%E3%80%82)

> 各个版本的下载：[https://code.jquery.com/](https://code.jquery.com/)

版本介绍：

> 1x ：兼容 IE 678 等低版本浏览器， 官网不再更新
>
> 2x ：不兼容 IE 678 等低版本浏览器， 官网不再更新
>
> 3x ：不兼容 IE 678 等低版本浏览器， 是官方主要更新维护的版本

### jQuery的入口函数

jQuery中常见的两种入口函数：

`// 第一种: 简单易用。 $(function () {        ...  // 此处是页面 DOM 加载完成的入口 }) ;  // 第二种: 繁琐，但是也可以实现 $(document).ready(function(){    ...  //  此处是页面DOM加载完成的入口 });`

总结：

1. 等着 DOM 结构渲染完毕即可执行内部代码，不必等到所有外部资源加载完成，jQuery 帮我们完成了封装。
2. 相当于原生 js 中的 DOMContentLoaded。
3. 不同于原生 js 中的 load 事件是等页面文档、外部的 js 文件、css文件、图片加载完毕才执行内部代码。
4. 更推荐使用第一种方式。

### jQuery中的顶级对象$

1. $是 jQuery 的别称，在代码中可以使用 jQuery 代替，但一般为了方便，通常都直接使用 $ 。
2. $是jQuery的顶级对象，相当于原生JavaScript中的 window。把元素利用$包装成jQuery对象，就可以调用jQuery 的方法。

### jQuery 对象和 DOM 对象

使用 jQuery 方法和原生JS获取的元素是不一样的，总结如下 :

1. 用原生 JS 获取来的对象就是 DOM 对象
2. jQuery 方法获取的元素就是 jQuery 对象。
3. jQuery 对象本质是： 利用$对DOM 对象包装后产生的对象（伪数组形式存储）。

> 注意：
>
> 只有 jQuery 对象才能使用 jQuery 方法，DOM 对象则使用原生的 JavaScirpt 方法。

![https://cloutp.github.io/wiki/web/jQuery/images/jQuery对象和DOM对象.png](https://cloutp.github.io/wiki/web/jQuery/images/jQuery%E5%AF%B9%E8%B1%A1%E5%92%8CDOM%E5%AF%B9%E8%B1%A1.png)

### jQuery 对象和 DOM 对象转换

DOM 对象与 jQuery 对象之间是可以相互转换的。因为原生js 比 jQuery 更大，原生的一些属性和方法 jQuery没有给我们封装. 要想使用这些属性和方法需要把jQuery对象转换为DOM对象才能使用。

`// 1.DOM对象转换成jQuery对象，方法只有一种 var box = document.getElementById('box');  // 获取DOM对象 var jQueryObject = $(box);  // 把DOM对象转换为 jQuery 对象 // 2.jQuery 对象转换为 DOM 对象有两种方法： //   2.1 jQuery对象[索引值] var domObject1 = $('div')[0] //   2.2 jQuery对象.get(索引值) var domObject2 = $('div').get(0)`

总结：实际开发比较常用的是把DOM对象转换为jQuery对象，这样能够调用功能更加强大的jQuery中的方法。

## 详细

- [选择器](/JavaScript/Libraries/jquery/选择器)
- [样式操作](/JavaScript/Libraries/jquery/样式操作)
- [属性操作](/JavaScript/Libraries/jquery/属性操作)
- [元素操作](/JavaScript/Libraries/jquery/元素操作)
- [事件操作](/JavaScript/Libraries/jquery/事件操作)
- [动画](/JavaScript/Libraries/jquery/动画)
- [其他](/JavaScript/Libraries/jquery/其他)