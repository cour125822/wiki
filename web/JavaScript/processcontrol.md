---
title: 流程控制
description: 
published: true
date: 2023-03-26T08:05:57.696Z
tags: 
editor: markdown
dateCreated: 2023-02-25T11:23:31.152Z
---

# 流程控制

### 顺序流程控制

顺序结构是程序中最简单、最基本的流程控制，它没有特定的语法结构，程序会按照代码的先后顺序，依次执行，程序中大多数的代码都是这样执行的。

### 分支流程控制

* 分支结构

由上到下执行代码的过程中，根据不同的条件，执行不同的路径代码（执行代码多选一的过程），从而得到不同的结果

JS 语言提供了两种分支结构语句：if 语句、switch 语句

* if 语句
* 语法结构

```
// 条件成立执行代码，否则什么也不做
if (条件表达式) {
    // 条件成立执行的代码语句
}
```

* if else语句（双分支语句）
* 语法结构

  ```
  // 条件成立  执行 if 里面代码，否则执行else 里面的代码
  if (条件表达式) {
      // [如果] 条件成立执行的代码
  } else {
      // [否则] 执行的代码
  }
  ```
* if else if 语句(多分支语句)
* 语法结构

  ```
  // 适合于检查多重条件。
  if (条件表达式1) {
      语句1；
  } else if (条件表达式2)  {
      语句2；
  } else if (条件表达式3)  {
     语句3；
   ....
  } else {
      // 上述条件都不成立执行此处代码
  }
  ```

### 三元表达式

* 语法结构

```
表达式1 ? 表达式2 : 表达式3;
```

* 执行思路
* 如果表达式1为 true ，则返回表达式2的值，如果表达式1为 false，则返回表达式3的值
* 简单理解： 就类似于 if else （双分支） 的简写

### switch分支流程控制

* 语法结构

switch 语句也是多分支语句，它用于基于不同的条件来执行不同的代码。当要针对变量设置一系列的特定值的选项时，就可以使用 switch。

```
switch( 表达式 ){
    case value1:
        // 表达式 等于 value1 时要执行的代码
        break;
    case value2:
        // 表达式 等于 value2 时要执行的代码
        break;
    default:
        // 表达式 不等于任何一个 value 时要执行的代码
}
```

* switch ：开关 转换 ， case ：小例子 选项
* 关键字 switch 后面括号内可以是表达式或值， 通常是一个变量
* 关键字 case , 后跟一个选项的表达式或值，后面跟一个冒号
* switch 表达式的值会与结构中的 case 的值做比较
* 如果存在匹配全等(===) ，则与该 case 关联的代码块会被执行，并在遇到 break 时停止，整个 switch 语句代码执行结束
* 如果所有的 case 的值都和表达式的值不匹配，则执行 default 里的代码**注意： 执行case 里面的语句时，如果没有break，则继续执行下一个case里面的语句。**
* switch 语句和 if else if 语句的区别
* 一般情况下，它们两个语句可以相互替换
* switch…case 语句通常处理 case为比较确定值的情况， 而 if…else…语句更加灵活，常用于范围判断(大于、等于某个范围)
* switch 语句进行条件判断后直接执行到程序的条件语句，效率更高。而if…else 语句有几种条件，就得判断多少次。
* 当分支比较少时，if… else语句的执行效率比 switch语句高。
* 当分支比较多时，switch语句的执行效率比较高，而且结构更清晰。

### for循环

* 语法结构

```
for(初始化变量; 条件表达式; 操作表达式 ){
    //循环体
}
```

| 名称       | 作用                                                                                            |
| ------------ | ------------------------------------------------------------------------------------------------- |
| 初始化变量 | 通常被用于初始化一个计数器，该表达式可以使用 var 关键字声明新的变量，这个变量帮我们来记录次数。 |
| 条件表达式 | 用于确定每一次循环是否能被执行。如果结果是 true 就继续循环，否则退出循环。                      |
| 操作表达式 | 用于确定每一次循环是否能被执行。如果结果是 true 就继续循环，否则退出循环。                      |

### 双重for循环

* 双重 for 循环概述

循环嵌套是指在一个循环语句中再定义一个循环语句的语法结构，例如在for循环语句中，可以再嵌套一个for 循环，这样的 for 循环语句我们称之为双重for循环。

* 双重 for 循环语法

```
for (外循环的初始; 外循环的条件; 外循环的操作表达式) {
    for (内循环的初始; 内循环的条件; 内循环的操作表达式) {
       需执行的代码;
   }
}
```

* 内层循环可以看做外层循环的循环体语句
* 内层循环执行的顺序也要遵循 for 循环的执行顺序
* 外层循环执行一次，内层循环要执行全部次数

### while循环

while语句的语法结构如下：

```
while (条件表达式) {
    // 循环体代码
}
```

执行思路：

* 1 先执行条件表达式，如果结果为 true，则执行循环体代码；如果为 false，则退出循环，执行后面代码
* 2 执行循环体代码
* 3 循环体代码执行完毕后，程序会继续判断执行条件表达式，如条件仍为true，则会继续执行循环体，直到循环条件为 false 时，整个循环过程才会结束

注意：

* 使用 while 循环时一定要注意，它必须要有退出条件，否则会成为死循环

### do-while循环

do… while 语句的语法结构如下：

```
do {
    // 循环体代码 - 条件表达式为 true 时重复执行循环体代码
} while(条件表达式);
```

执行思路

* 1 先执行一次循环体代码
* 2 再执行条件表达式，如果结果为 true，则继续执行循环体代码，如果为 false，则退出循环，继续执行后面代码

注意：先再执行循环体，再判断，do…while循环语句至少会执行一次循环体代码

### continue、break

continue 关键字用于立即跳出本次循环，继续下一次循环（本次循环体中 continue 之后的代码就会少执行一次）。

break 关键字用于立即跳出整个循环（循环结束）。