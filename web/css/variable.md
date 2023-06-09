---
title: CSS变量
description: 
published: true
date: 2023-03-26T08:07:01.402Z
tags: 
editor: markdown
dateCreated: 2023-02-25T10:58:26.554Z
---

## var() 函数的语法

var() 函数用于插入 CSS 变量的值。

var() 函数的语法如下：

```
var(name, value)
```

| 值 | 描述                                 |
| ---- | -------------------------------------- |
| *name*   | 必需。变量名（以两条破折号开头）。   |
| *value*   | 可选。回退值（在未找到变量时使用）。 |

注释：变量名称必须以两个破折号（--）开头，且区分大小写！

## var() 如何工作

首先：CSS 变量可以有全局或局部作用域。

全局变量可以在整个文档中进行访问/使用，而局部变量只能在声明它的选择器内部使用。

如需创建具有全局作用域的变量，请在 :root 选择器中声明它。 :root 选择器匹配文档的根元素。

如需创建具有局部作用域的变量，请在将要使用它的选择器中声明它。

下面的例子与上面的例子相同，但是在这里我们使用 var() 函数。

首先，我们声明两个全局变量（--blue 和 --white）。然后，我们使用 var() 函数稍后在样式表中插入变量的值：

### 实例

```
:root {
  --blue: #1e90ff;
  --white: #ffffff;
}

body { background-color: var(--blue); }

h2 { border-bottom: 2px solid var(--blue); }

.container {
  color: var(--blue);
  background-color: var(--white);
  padding: 15px;
}

button {
  background-color: var(--white);
  color: var(--blue);
  border: 1px solid var(--blue);
  padding: 5px;
}
```

[亲自试一试](https://www.w3school.com.cn/tiy/t.asp?f=css3_var_1)

使用 var() 有如下优势：

* 使代码更易于阅读（更容易理解）
* 使修改颜色值更加容易

如需将蓝色和白色改为较柔和的蓝色和白色，您只需要修改两个变量值：

### 实例

```
:root {
  --blue: #6495ed;
  --white: #faf0e6;
}

body { background-color: var(--blue); }

h2 { border-bottom: 2px solid var(--blue); }

.container {
  color: var(--blue);
  background-color: var(--white);
  padding: 15px;
}

button {
  background-color: var(--white);
  color: var(--blue);
  border: 1px solid var(--blue);
  padding: 5px;
}
```

>  局部变量会覆盖全局变量

## 使用 JavaScript 更改变量

CSS 变量可以访问 DOM，这意味着您可以通过 JavaScript 更改它们。

这个例子说明了如何创建脚本来显示并更改上一页中使用的示例中的 --blue 变量。此刻，如果您不熟悉 JavaScript，不要担心。您可以在我们的 JavaScript 教程中学到有关 JavaScript 的更多知识：

### 实例

```
<script>
// 获取根元素
var r = document.querySelector(':root');

// 创建获取变量值的函数
function myFunction_get() {
  // Get the styles (properties and values) for the root
  var rs = getComputedStyle(r);
  // Alert the value of the --blue variable
  alert("The value of --blue is: " + rs.getPropertyValue('--blue'));
}

// 创建设置变量值的函数
function myFunction_set() {
  // Set the value of variable --blue to another value (in this case "lightblue")
  r.style.setProperty('--blue', 'lightblue');
}
</script>
```