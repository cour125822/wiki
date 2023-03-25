---
title: 单位
description: 
published: true
date: 2023-03-06T03:06:31.544Z
tags: 
editor: markdown
dateCreated: 2023-02-25T11:01:07.033Z
---

# CSS 单位

CSS 有几种表示长度的不同单位。

许多 CSS 属性接受“长度”值，诸如 `width`、`margin`、`padding`、`font-size` 等。

长度是一个后面跟着长度单位的数字，诸如 `10px`、`2em` 等。

### 实例

使用 px（像素）设置不同的长度值：

```
h1 {
  font-size: 60px;
}

p {
  font-size: 25px;
  line-height: 50px;
}
```

[亲自试一试](https://www.w3school.com.cn/tiy/t.asp?f=css_units_px)

数字和单位之间不能出现空格。但是，如果值为 0，则可以省略单位。

对于某些 CSS 属性，允许使用负的长度。

长度单位有两种类型：*绝对单位*和*相对单位*。

## 绝对长度

绝对长度单位是固定的，用任何一个绝对长度表示的长度都将恰好显示为这个尺寸。

不建议在屏幕上使用绝对长度单位，因为屏幕尺寸变化很大。但是，如果已知输出介质，则可以使用它们，例如用于打印布局（print layout）。

| 单位 | 描述                       | TIY |
| ------ | ---------------------------- | ----- |
| cm   | 厘米                       | [https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_cm](https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_cm)    |
| mm   | 毫米                       | [https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_mm](https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_mm)    |
| in   | 英寸 (1in = 96px = 2.54cm) | [https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_in](https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_in)    |
| px * | 像素 (1px = 1/96th of 1in) | [https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_px](https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_px)    |
| pt   | 点 (1pt = 1/72 of 1in)     | [https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_pt](https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_pt)    |
| pc   | 派卡 (1pc = 12 pt)         | [https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_pc](https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_pc)    |

* 像素（px）是相对于观看设备的。对于低 dpi 的设备，1px 是显示器的一个设备像素（点）。对于打印机和高分辨率屏幕，1px 表示多个设备像素。

## 相对长度

相对长度单位规定相对于另一个长度属性的长度。相对长度单位在不同渲染介质之间缩放表现得更好。

| 单位 | 描述                                                             | TIY |
| ------ | ------------------------------------------------------------------ | ----- |
| em   | 相对于元素的字体大小（font-size）（2em 表示当前字体大小的 2 倍） | [https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_em](https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_em)    |
| ex   | 相对于当前字体的 x-height(极少使用)                              | [https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_ex](https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_ex)    |
| ch   | 相对于 “0”（零）的宽度                                         | [https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_ch](https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_ch)    |
| rem  | 相对于根元素的字体大小（font-size）                              | [https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_rem](https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_rem)    |
| vw   | 相对于视口*宽度的 1%                                             | [https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_vw](https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_vw)    |
| vh   | 相对于视口*高度的 1%                                             | [https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_vh](https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_vh)    |
| vmin | 相对于视口*较小尺寸的 1％                                        | [https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_vmin](https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_vmin)    |
| vmax | 相对于视口*较大尺寸的 1％                                        | [https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_vmax](https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_vmax)    |
| %    | 相对于父元素                                                     | [https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_percentage](https://www.w3school.com.cn/tiy/t.asp?f=cssref_unit_percentage)    |

提示：em 和 rem 单元可用于创建完美的可扩展布局！

* 视口（Viewport）= 浏览器窗口的尺寸。如果视口宽 50 里面，则 1vw = 0.5cm。

## 浏览器支持

表格中的数字注明了完全支持该长度单位的首个浏览器版本。

| 长度单位                          |      |      |      |     |      |
| ----------------------------------- | ------ | ------ | ------ | ----- | ------ |
| em, ex, %, px, cm, mm, in, pt, pc | 1.0  | 3.0  | 1.0  | 1.0 | 3.5  |
| ch                                | 27.0 | 9.0  | 1.0  | 7.0 | 20.0 |
| rem                               | 4.0  | 9.0  | 3.6  | 4.1 | 11.6 |
| vh, vw                            | 20.0 | 9.0  | 19.0 | 6.0 | 20.0 |
| vmin                              | 20.0 | 12.0 | 19.0 | 6.0 | 20.0 |
| vmax                              | 26.0 | 16.0 | 19.0 | 7.0 | 20.0 |

* [CSS 字体回退](https://www.w3school.com.cn/cssref/css_fonts_fallbacks.asp)
* [CSS 颜色](https://www.w3school.com.cn/cssref/css_colors.asp)

### [CSS 参考手册](https://www.w3school.com.cn/cssref/index.asp)

### [CSS 实例](https://www.w3school.com.cn/css/css_examples.asp)

### [CSS 测验](https://www.w3school.com.cn/css/css_quiz.asp)

### [CSS 教程](https://www.w3school.com.cn/css/index.asp)