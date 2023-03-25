---
title: 闭包
description: 
published: true
date: 2023-03-06T03:16:06.919Z
tags: 
editor: markdown
dateCreated: 2023-02-25T11:25:26.544Z
---

# 闭包

## 闭包

变量根据作用域的不同分为两种：全局变量和局部变量。

1. 函数内部可以使用全局变量。
2. 函数外部不可以使用局部变量。
3. 当函数执行完毕，本作用域内的局部变量会销毁。

### 什么是闭包

闭包（closure）指有权访问另一个函数作用域中变量的函数。简单理解就是 ，一个作用域可以访问另外一个函数内部的局部变量。

```
<script>
    function fn1( ){    // fn1就是闭包函数
        var num = 10;
        function fn2 ( ){
            console.log (num) ; // 10
        }
        fn2 ()
    }
    fn1 ( ) ;
</ script>
```

### 闭包的作用

作用：延伸变量的作用范围。

```
 function fn() {
   var num = 10;
   function fun() {
       console.log(num);
    }
    return fun;
 }
var f = fn();
f();
```

### 闭包的案例

1. 利用闭包的方式得到当前li 的索引号

```
for (var i = 0; i < lis.length; i++) {
// 利用for循环创建了4个立即执行函数
// 立即执行函数也成为小闭包因为立即执行函数里面的任何一个函数都可以使用它的i这变量
(function(i) {
    lis[i].onclick = function() {
      console.log(i);
    }
 })(i);
}
```

1. 闭包应用-3秒钟之后,打印所有li元素的内容

```
 for (var i = 0; i < lis.length; i++) {
   (function(i) {
     setTimeout(function() {
     console.log(lis[i].innerHTML);
     }, 3000)
   })(i);
}
```

1. 闭包应用-计算打车价格

```
/*需求分析
打车起步价13(3公里内),  之后每多一公里增加 5块钱.  用户输入公里数就可以计算打车价格
如果有拥堵情况,总价格多收取10块钱拥堵费*/

 var car = (function() {
     var start = 13; // 起步价  局部变量
     var total = 0; // 总价  局部变量
     return {
       // 正常的总价
       price: function(n) {
         if (n <= 3) {
           total = start;
         } else {
           total = start + (n - 3) * 5
         }
         return total;
       },
       // 拥堵之后的费用
       yd: function(flag) {
         return flag ? total + 10 : total;
       }
    }
 })();
console.log(car.price(5)); // 23
console.log(car.yd(true)); // 33
```

### 案例

```
 var name = "The Window";
   var object = {
     name: "My Object",
     getNameFunc: function() {
     return function() {
     return this.name;
     };
   }
 };
console.log(object.getNameFunc()())
-----------------------------------------------------------------------------------
var name = "The Window";
  var object = {
    name: "My Object",
    getNameFunc: function() {
    var that = this;
    return function() {
    return that.name;
    };
  }
};
console.log(object.getNameFunc()())
```

‍