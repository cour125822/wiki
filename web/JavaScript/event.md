---
title: 事件
description: 
published: true
date: 2023-03-26T08:05:44.603Z
tags: 
editor: markdown
dateCreated: 2023-02-25T11:34:43.609Z
---

**触发--- 响应机制**。

网页中的每个元素都可以产生某些可以触发 JavaScript 的事件，例如，我们可以在用户点击某按钮时产生一个 事件，然后去执行某些操作。

### 事件三要素

* 事件源（谁）：触发事件的元素
* 事件类型（什么事件）： 例如 click 点击事件
* 事件处理程序（做啥）：事件触发后要执行的代码(函数形式)，事件处理函数

### 执行事件的步骤

1. 获取事件源
2. 注册事件(绑定事件)
3. 添加事件处理程序(采取函数赋值形式)

### 注册事件（2种方式）

**Node**

给元素添加事件，称为注册事件或者绑定事件。 注册事件有两种方式:传统方式和监听注册方式

**传统注册方式**

* 利用on开头的事件onclick
* `<button onclick= "alert("hi~')” ></button>`
* ``btn.onclick = function() {}`
* 特点:注册事件的唯一性
* 同—个元素同—个事件只能设置一个处理函数，最后注册的处理函数将会覆盖前面注册的处理函数

**监听注册方式**

* w3c标准推荐方式
* `addEventListener()`它是一个方法
* IE9之前的IE不支持此方法，可使用`attachEvent()`代替
* 特点:同一个元素同一个事件可以注册多个监听器
* 按注册顺序依次执行

### 事件监听

### **addEventListener()事件监听（IE9以后支持）**

`eventTarget.addEventListener(type,listener[, useCapture])`

`eventTarget.addEventListener()`方法将指定的监听器注册到 `eventTarget`（目标对象）上，当该对象触发指定的事件时，就会执行事件处理函数。

**该方法接收三个参数:**

* type:事件类型字符串，比如click、mouseover，注意这里不要带on
* listener:事件处理函数，事件发生时，会调用该监听函数
* useCapture:可选参数，是一个布尔值，默认是false。事件的触发方式

### **attacheEvent()事件监听（IE678支持）**

`eventTarget.attachEvent(eventNamewithon,callback)`

`eventTarget.attachEvent()`方法将指定的监听器注册到 `eventTarget`（目标对象） 上，当该对象触发指定的事件时，指定的回调函数就会被执行。

**该方法接收两个参数:**

* eventNameWithon:事件类型字符串，比如onclick、onmouseover，这里要带on
* callback:事件处理函数，当目标触发事件时回调函数被调用

**注意:**IE8及早期版本支持

`<button>传统注册事件</button> <button>方法监听注册事件</button> <button>ie9 attachEvent</button> <script>     var btns = document.querySelectorAll('button');     // 1. 传统方式注册事件     btns[0].onclick = function() {         alert('hi');     }     btns[0].onclick = function() {             alert('hao a u');         }    // 2. 事件侦听注册事件 addEventListener     // (1) 里面的事件类型是字符串 必定加引号 而且不带on    // (2) 同一个元素 同一个事件可以添加多个侦听器（事件处理程序）     btns[1].addEventListener('click', function() {         alert(22);     })     btns[1].addEventListener('click', function() {             alert(33);     })     // 3. attachEvent ie9以前的版本支持     btns[2].attachEvent('onclick', function() {         alert(11);     }) </script>`

### **事件监听兼容性解决方案**

封装一个函数，函数中判断浏览器的类型：

`function addEventListener(element,eventName,fn){     //判断当前浏览器是否支持addEventListener方法     if (element.addEventListener) {         element.addEventListener(eventName,fn);         //第三个参数默认是false     }else if (element.attachEvent){         element.attachEvent ( 'on' + eventName, fn) ;     }else {         //相当于element.onclick = fn;         element [ 'on' + eventName] = fn; }`

### 删除事件（解绑事件）

1. 传统注册方式

`eventTarget.onclick =null;`

1. 方法监听注册方式
2. `eventTarget.removeEventListener(type，listener[,useCapture] ) ;`
3. `eventTarget.detachEvent (eventNamewithon,callback) ;<div>1</div>  <div>2</div>  <div>3</div>  <script>      var divs = document.querySelectorAll('div');      divs[0].onclick = function() {          alert(11);          // 1. 传统方式删除事件          divs[0].onclick = null;      }      // 2. removeEventListener 删除事件      divs[1].addEventListener('click', fn) // 里面的fn 不需要调用加小括号      function fn() {          alert(22);          divs[1].removeEventListener('click', fn);      }      // 3. detachEvent      divs[2].attachEvent('onclick', fn1);      function fn1() {          alert(33);          divs[2].detachEvent('onclick', fn1);      }  </script>`

**删除事件兼容性解决方案**

`function removeEventListener(element,eventName，fn){     //判断当前浏览器是否支持removeEventListener方法     if (element.removeEventListener) {         element.removeEventListener(eventName，fn); //第三个参数默认是false     }else if (element.detachEvent){         element.detachEvent ( ' on' + eventName, fn) ;     } else {         element [ ' on' + eventName] = null;     } }`


### 事件操作
- [事件流](/JavaScript/event/事件流)
- [事件对象](/JavaScript/event/事件对象)
- [事件委托](/JavaScript/event/事件委托)
- [鼠标事件](/JavaScript/event/鼠标事件)
- [键盘事件](/JavaScript/event/键盘事件)
- [触屏事件](/JavaScript/event/触屏事件)

