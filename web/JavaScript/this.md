---
title: this指向
description: 
published: true
date: 2023-03-26T08:06:02.485Z
tags: 
editor: markdown
dateCreated: 2023-02-25T11:25:59.565Z
---

# this

### 函数内部的this指向

这些 this 的指向，是当我们调用函数的时候确定的。调用方式的不同决定了this 的指向不同

一般指向我们的调用者.

| 调用方式     | this指向                                 |
| -------------- | ------------------------------------------ |
| 普通函数调用 | window                                   |
| 构造函数调用 | 实例对象原型对象里面的方法也指向实例对象 |
| 对象方法调用 | 该方法所属对象                           |
| 事件绑定方法 | 绑定事件对象                             |
| 定时器函数   | window                                   |
| 立即执行函数 | window                                   |
|              |                                          |

### 改变函数内部 this 指向

### call方法

call()方法调用一个对象。简单理解为调用函数的方式，但是它可以改变函数的 this 指向

应用场景: 经常做继承.

```
var o = {
    name: 'andy'
}
 function fn(a, b) {
      console.log(this);
      console.log(a+b)
};
fn(1,2)// 此时的this指向的是window 运行结果为3
fn.call(o,1,2)//此时的this指向的是对象o,参数使用逗号隔开,运行结果为3
```

### apply方法

apply() 方法调用一个函数。简单理解为调用函数的方式，但是它可以改变函数的 this 指向。

应用场景: 经常跟数组有关系

```
var o = {
    name: 'andy'
}
 function fn(a, b) {
      console.log(this);
      console.log(a+b)
};
fn()// 此时的this指向的是window 运行结果为3
fn.apply(o,[1,2])//此时的this指向的是对象o,参数使用数组传递 运行结果为3
```

### bind方法

bind() 方法不会调用函数,但是能改变函数内部this 指向,返回的是原函数改变this之后产生的新函数

如果只是想改变 this 指向，并且不想调用这个函数的时候，可以使用bind

应用场景:不调用函数,但是还想改变this指向

```
 var o = {
 name: 'andy'
 };

function fn(a, b) {
    console.log(this);
    console.log(a + b);
};
var f = fn.bind(o, 1, 2); //此处的f是bind返回的新函数
f();//调用新函数  this指向的是对象o 参数使用逗号隔开
```

### call、apply、bind三者的异同

* 共同点 : 都可以改变this指向
* 不同点:
* call 和 apply 会调用函数, 并且改变函数内部this指向.
* call 和 apply传递的参数不一样,call传递参数使用逗号隔开,apply使用数组传递
* bind 不会调用函数, 可以改变函数内部this指向.
* 应用场景
* call 经常做继承.
* apply经常跟数组有关系. 比如借助于数学对象实现数组最大值最小值
* bind 不调用函数,但是还想改变this指向. 比如改变定时器内部的this指向.

[this指向问题](https://www.notion.so/this-f7abd774935b44b6be3c8772ebba7fb7)




### 

this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁，一般情况下this的最终指向的是那个调用它的对象。

现阶段，我们先了解一下几个this指向

1. 全局作用域或者普通函数中this指向全局对象window（注意定时器里面的this指向window）
2. 方法调用中谁调用this指向谁
3. 构造函数中this指向构造函数的实例
    `<button>点击</button>  <script>      // this 指向问题 一般情况下this的最终指向的是那个调用它的对象      // 1. 全局作用域或者普通函数中this指向全局对象window（ 注意定时器里面的this指向window）      console.log(this);      function fn() {          console.log(this);      }      window.fn();      window.setTimeout(function() {          console.log(this);      }, 1000);      // 2. 方法调用中谁调用this指向谁      var o = {          sayHi: function() {              console.log(this); // this指向的是 o 这个对象          }      }      o.sayHi();      var btn = document.querySelector('button');      btn.addEventListener('click', function() {              console.log(this); // 事件处理函数中的this指向的是btn这个按钮对象          })      // 3. 构造函数中this指向构造函数的实例      function Fun() {          console.log(this); // this 指向的是fun 实例对象      }      var fun = new Fun();  </script>`