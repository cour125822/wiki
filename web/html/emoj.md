---
title: 表情符号
description: 
published: true
date: 2023-03-26T08:07:06.123Z
tags: 
editor: markdown
dateCreated: 2023-02-25T10:27:27.156Z
---

# 在 HTML 中使用表情符号

* [HTML 符号](https://www.w3school.com.cn/html/html_symbols.asp)
* [HTML 字符集](https://www.w3school.com.cn/html/html_charset.asp)

表情符号（Emoji）是来自 UTF-8 字符集的字符：😄 😍 💗

## 什么是 Emoji？

表情符号（Emoji）类似图像或图标，但它们并不是。

它们是来自 UTF-8 (Unicode) 字符集的字母（字符）。

提示：UTF-8 几乎涵盖世界上所有字符和符号。

## HTML charset 属性

为了正确显示 HTML 页面，Web 浏览器必须知道页面中使用的字符集。

这是在 `<meta>` 标签中规定的：

```
<meta charset="UTF-8">
```

提示：如果未规定，UTF-8 则是 HTML 中的默认字符集。

## UTF-8 字符

很多 UTF-8 字符无法在键盘上键入，但始终可以使用数字（被称为实体编号）来显示它们：

* A 是 65
* B 是 66
* C 是 67

### 实例

```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
</head>
<body>

<p>我将显示 A B C</p>
<p>我将显示 &#65; &#66; &#67;</p>

</body>
</html>
```

[亲自试一试](https://www.w3school.com.cn/tiy/t.asp?f=html_emoji_utf8)

### 例子解释

`<meta charset="UTF-8">` 元素定义字符集。

字符 A、B、C 由数字 65、66 以及 67 来显示。

为了让浏览器了解您正在显示字符，您必须以 &# 开头并以 ;（分号）结束实体编号。

## Emoji 字符

表情符号也是来自 UTF-8 字母的字符：

* 😄 是 128516
* 😍 是 128525
* 💗 是 128151

### 实例

```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
</head>
<body>

<h1>我的第一个 Emoji</h1>

<p>&#128512;</p>

</body>
</html>
```

[亲自试一试](https://www.w3school.com.cn/tiy/t.asp?f=html_emoji)

由于表情符号是字符，因此可以像 HTML 中的其他任何字符一样复制、显示和调整它们的大小。

### 实例

```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
</head>
<body>

<h1>放大的表情符号</h1>

<p style="font-size:48px">
&#128512; &#128516; &#128525; &#128151;
</p>

</body>
</html>
```

[亲自试一试](https://www.w3school.com.cn/tiy/t.asp?f=html_emoji_size)

## UTF-8 中的一些 Emoji 符号

| Emoji | 值 |
| ------- | ---- |
| 🗻    | 🗻 |
| 🗼    | 🗼 |
| 🗽    | 🗽 |
| 🗾    | 🗾 |
| 🗿    | 🗿 |
| 😀    | 😀 |
| 😁    | 😁 |
| 😂    | 😂 |
| 😃    | 😃 |
| 😄    | 😄 |
| 😅    | 😅 |

如需完整列表，请 HTML Emoji 参考手册