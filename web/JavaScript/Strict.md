---
title: 严格模式
description: 
published: true
date: 2023-03-26T08:05:28.588Z
tags: 
editor: markdown
dateCreated: 2023-02-25T11:32:52.406Z
---

# 严格模式

### 什么是严格模式

JavaScript 除了提供正常模式外，还提供了严格模式（strict mode）。ES5 的严格模式是采用具有限制性 JavaScript变体的一种方式，即在严格的条件下运行 JS 代码。

严格模式在 IE10 以上版本的浏览器中才会被支持，旧版本浏览器中会被忽略。

严格模式对正常的 JavaScript 语义做了一些更改：

1.消除了 Javascript 语法的一些不合理、不严谨之处，减少了一些怪异行为。

2.消除代码运行的一些不安全之处，保证代码运行的安全。

3.提高编译器效率，增加运行速度。

4.禁用了在 ECMAScript 的未来版本中可能会定义的一些语法，为未来新版本的 Javascript 做好铺垫。比如一些保留字如：class,enum,export, extends, import, super 不能做变量名

### 开启严格模式

严格模式可以应用到整个脚本或个别函数中。因此在使用时，我们可以将严格模式分为为脚本开启严格模式和为函数开启严格模式两种情况。

* 情况一 :为脚本开启严格模式
* 有的 script 脚本是严格模式，有的 script 脚本是正常模式，这样不利于文件合并，所以可以将整个脚本文件放在一个立即执行的匿名函数之中。这样独立创建一个作用域而不影响其他 script 脚本文件。

  ```
  (function (){
    //在当前的这个自调用函数中有开启严格模式，当前函数之外还是普通模式
  　　　　"use strict";
         var num = 10;
  　　　　function fn() {}
  })();
  //或者
  <script>
    　"use strict"; //当前script标签开启了严格模式
  </script>
  <script>
              //当前script标签未开启严格模式
  </script>
  ```
* 情况二: 为函数开启严格模式
* 要给某个函数开启严格模式，需要把“use strict”; (或 ‘use strict’; ) 声明放在函数体所有语句之前。

  ```
  function fn(){
  　　"use strict";
  　　return "123";
  }
  //当前fn函数开启了严格模式
  ```

### 严格模式中的变化

严格模式对 Javascript 的语法和行为，都做了一些改变。

```
'use strict'
num = 10
console.log(num)//严格模式后使用未声明的变量
--------------------------------------------------------------------------------
var num2 = 1;
delete num2;//严格模式不允许删除变量
--------------------------------------------------------------------------------
function fn() {
 console.log(this); // 严格模式下全局作用域中函数中的 this 是 undefined
}
fn();
---------------------------------------------------------------------------------
function Star() {
     this.sex = '男';
}
// Star();严格模式下,如果 构造函数不加new调用, this 指向的是undefined 如果给他赋值则 会报错.
var ldh = new Star();
console.log(ldh.sex);
----------------------------------------------------------------------------------
setTimeout(function() {
  console.log(this); //严格模式下，定时器 this 还是指向 window
}, 2000);
```

[更多严格模式要求参考](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode)