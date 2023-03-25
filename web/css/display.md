---
title: CSS布局
description: 
published: true
date: 2023-03-06T03:05:33.798Z
tags: 
editor: markdown
dateCreated: 2023-02-25T10:57:25.057Z
---

## 元素属性

| 属性      | 描述                               |
| ----------- | ------------------------------------ |
| [display](https://www.w3school.com.cn/cssref/pr_class_display.asp "CSS display 属性")          | 指定应如何显示元素。               |
| [visibility](https://www.w3school.com.cn/cssref/pr_class_visibility.asp "CSS visibility 属性")          | 指定元素是否应该可见。             |
| width     |                                    |
| max-width |                                    |
| margin    |                                    |
| position  |  staticrelativefixedabsolutesticky |
|           |                                    |

### 分类

* 行级元素
* 块级元素
* 行级块元素


## 定位

## 定位属性

| 属性 | 描述                         |
| ------ | ------------------------------ |
| [bottom](https://www.w3school.com.cn/cssref/pr_pos_bottom.asp "CSS bottom 属性")     | 设置定位框的底部外边距边缘。 |
| [clip](https://www.w3school.com.cn/cssref/pr_pos_clip.asp "CSS clip 属性")     | 剪裁绝对定位的元素。         |
| [left](https://www.w3school.com.cn/cssref/pr_pos_left.asp "CSS left 属性")     | 设置定位框的左侧外边距边缘。 |
| [position](https://www.w3school.com.cn/cssref/pr_class_position.asp "CSS position 属性")     | 规定元素的定位类型。         |
| [right](https://www.w3school.com.cn/cssref/pr_pos_right.asp "CSS right 属性")     | 设置定位框的右侧外边距边缘。 |
| [top](https://www.w3school.com.cn/cssref/pr_pos_top.asp "CSS top 属性")     | 设置定位框的顶部外边距边缘。 |
| [z-index](https://www.w3school.com.cn/cssref/pr_pos_z-index.asp "CSS z-index 属性")     | 设置元素的堆叠顺序。         |

### position: static;

HTML 元素默认情况下的定位方式为 static（静态）。

静态定位的元素不受 top、bottom、left 和 right 属性的影响。

position: static; 的元素不会以任何特殊方式定位；它始终根据页面的正常流进行定位：

这个 <div> 元素设置了 position: static;

### position: relative;

`position: relative;` 的元素相对于其正常位置进行定位。

设置相对定位的元素的 top、right、bottom 和 left 属性将导致其偏离其正常位置进行调整。不会对其余内容进行调整来适应元素留下的任何空间。

这个 <div> 元素设置了 position: relative;

### position: fixed;

`position: fixed;` 的元素是相对于视口定位的，这意味着即使滚动页面，它也始终位于同一位置。 top、right、bottom 和 left 属性用于定位此元素。

固定定位的元素不会在页面中通常应放置的位置上留出空隙。

### position: absolute;

`position: absolute;` 的元素相对于最近的定位祖先元素进行定位（而不是相对于视口定位，如 fixed）。

然而，如果绝对定位的元素没有祖先，它将使用文档主体（body），并随页面滚动一起移动。

注意：“被定位的”元素是其位置除 `static` 以外的任何元素。

这是一个简单的例子：

这个 <div> 元素设置了 position: relative;这个 <div> 元素设置了 position: absolute;

### position: sticky;

`position: sticky;` 的元素根据用户的滚动位置进行定位。

粘性元素根据滚动位置在相对（`relative`）和固定（`fixed`）之间切换。起先它会被相对定位，直到在视口中遇到给定的偏移位置为止 - 然后将其“粘贴”在适当的位置（比如 position:fixed）。

<iframe src="https://www.w3school.com.cn/demo/css/position_sticky_ifr.html" data-src="https://www.w3school.com.cn/demo/css/position_sticky_ifr.html"></iframe>


注意：Internet Explorer、Edge 15 以及更早的版本不支持粘性定位。 Safari 需要 -webkit- 前缀（请参见下面的实例）。您还必须至少指定 `top`、`right`、`bottom` 或 `left` 之一，以便粘性定位起作用。

### 重叠元素

在对元素进行定位时，它们可以与其他元素重叠。

`z-index` 属性指定元素的堆栈顺序（哪个元素应放置在其他元素的前面或后面）。

元素可以设置正或负的堆叠顺序：

### 这是一个标题

![](https://www.w3school.com.cn/i/logo/w3logo-4.png)由于图像的 z-index 为 -1，所以它位于文本后面。

### 实例

```
img {
  position: absolute;
  left: 0px;
  top: 0px;
  z-index: -1;
}
```

[亲自试一试](https://www.w3school.com.cn/tiy/t.asp?f=css_zindex)

具有较高堆叠顺序的元素始终位于具有较低堆叠顺序的元素之前。

注意：如果两个定位的元素重叠而未指定 `z-index`，则位于 HTML 代码中最后的元素将显示在顶部。

## 溢出

| 属性 | 描述                                                |
| ------ | ----------------------------------------------------- |
| [overflow](https://www.w3schools.com/cssref/pr_pos_overflow.asp "CSS overflow 属性")     | 规定如果内容溢出元素框会发生什么情况。              |
| [overflow-x](https://www.w3schools.com/cssref/css3_pr_overflow-x.asp "CSS overflow-x 属性")     | 规定在元素的内容区域溢出时如何处理内容的左/右边缘。 |
| [overflow-y](https://www.w3schools.com/cssref/css3_pr_overflow-y.asp "CSS overflow-y 属性")     | 指定在元素的内容区域溢出时如何处理内容的上/下边缘。 |

* `visible` - 默认。溢出没有被剪裁。内容在元素框外渲染
* `hidden` - 溢出被剪裁，其余内容将不可见
* `scroll` - 溢出被剪裁，同时添加滚动条以查看其余内容
* `auto` - 与 `scroll` 类似，但仅在必要时添加滚动条

> `overflow` 属性仅适用于具有指定高度的块元素。

## 浮动

`float` 属性用于定位和格式化内容，例如让图像向左浮动到容器中的文本那里。

`float` 属性可以设置以下值之一：

* left - 元素浮动到其容器的左侧
* right - 元素浮动在其容器的右侧
* none - 元素不会浮动（将显示在文本中刚出现的位置）。默认值。
* inherit - 元素继承其父级的 float 值

### 清除浮动

`clear` 属性指定哪些元素可以浮动于被清除元素的旁边以及哪一侧。

`clear` 属性可设置以下值之一：

* none - 允许两侧都有浮动元素。默认值
* left - 左侧不允许浮动元素
* right- 右侧不允许浮动元素
* both - 左侧或右侧均不允许浮动元素
* inherit - 元素继承其父级的 clear 值

使用 `clear` 属性的最常见用法是在元素上使用了 `float` 属性之后。

在清除浮动时，应该对清除与浮动进行匹配：如果某个元素浮动到左侧，则应清除左侧。您的浮动元素会继续浮动，但是被清除的元素将显示在其下

`clearfix Hack`

如果一个元素比包含它的元素高，并且它是浮动的，它将“溢出”到其容器之外：

然后我们可以向包含元素添加 overflow: auto;，来解决此问题：

###