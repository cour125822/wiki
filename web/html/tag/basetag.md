---
title: 基础标签
description: 
published: true
date: 2023-03-26T08:09:46.859Z
tags: 
editor: markdown
dateCreated: 2023-02-25T10:30:24.993Z
---

# 文档声明
## 使用
```html
<!DOCTYPE>	<!-->声明没有结束标签,必须是 HTML 文档的第一行，位于所有标签之前。-->
```
>  声明不是 HTML 标签；它是指示 web 浏览器关于页面使用哪个 HTML 版本进行编写的指令。

在 HTML 4.01 中，<!DOCTYPE> 声明引用 DTD，因为 HTML 4.01 基于 SGML。DTD 规定了标记语言的规则，这样浏览器才能正确地呈现内容。

HTML5 不基于 SGML，所以不需要引用 DTD。


## 常用的 DOCTYPE 声明

### HTML 5

```
<!DOCTYPE html>
```

### HTML 4.01 Strict

该 DTD 包含所有 HTML 元素和属性，但不包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "<http://www.w3.org/TR/html4/strict.dtd>">
```

### HTML 4.01 Transitional

该 DTD 包含所有 HTML 元素和属性，包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"<http://www.w3.org/TR/html4/loose.dtd>">
```

### HTML 4.01 Frameset

该 DTD 等同于 HTML 4.01 Transitional，但允许框架集内容。

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN"
"<http://www.w3.org/TR/html4/frameset.dtd>">
```

### XHTML 1.0 Strict

该 DTD 包含所有 HTML 元素和属性，但不包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。必须以格式正确的 XML 来编写标记。

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
"<http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd>">
```

### XHTML 1.0 Transitional

该 DTD 包含所有 HTML 元素和属性，包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。必须以格式正确的 XML 来编写标记。

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "
<http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd>">
```

### XHTML 1.0 Frameset

该 DTD 等同于 XHTML 1.0 Transitional，但允许框架集内容。

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN"
"<http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd>">
```

### XHTML 1.1

该 DTD 等同于 XHTML 1.0 Strict，但允许添加模型（例如提供对东亚语系的 ruby 支持）。

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "<http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd>">
```



> 此元素可告知浏览器其自身是一个 HTML 文档。

# HTML标签
```html
<html></html>
```
标签限定了文档的开始点和结束点，在它们之间是文档的头部和主体


## 属性

new : HTML5 中的新属性。

| 属性	| 值  | 描述                                              |
| ---------- | ----- | --------------------------------------------------- |
| manifest | url | 定义一个 URL，在这个 URL 上描述了文档的缓存信息。 |
| xmlns         | url  | 定义 XML namespace 属性。                         |

即使 html 元素是文档的根元素，它也不包含 doctype 元素。doctype 元素必须位于 html 元素之前。



# head标签

标签用于定义文档的头部，它是所有头部元素的容器。

中的元素可以引用脚本、指示浏览器在哪里找到样式表、提供元信息等等。

文档的头部描述了文档的各种属性和信息，包括文档的标题、在 Web 中的位置以及和其他文档的关系等。绝大多数文档头部包含的数据都不会真正作为内容显示给读者。

下面这些标签可用在 head 部分：[&lt;base&gt;](https://www.w3school.com.cn/tags/tag_base.asp), [&lt;link&gt;](https://www.w3school.com.cn/tags/tag_link.asp), [&lt;meta&gt;](https://www.w3school.com.cn/tags/tag_meta.asp), [&lt;script&gt;](https://www.w3school.com.cn/tags/tag_script.asp), [&lt;style&gt;](https://www.w3school.com.cn/tags/tag_style.asp), 以及 [&lt;title&gt;](https://www.w3school.com.cn/tags/tag_title.asp)。

`<title>`定义文档的标题，它是 head 部分中唯一必需的元素。

## 可选的属性

| 属性    | 值  | 描述                                                             |
| --------- | ----- | ------------------------------------------------------------------ |
| profile | URL | 一个由空格分隔的 URL 列表，这些 URL 包含着有关页面的元数据信息。 |

## profile 属性的更多信息

文档的头部经常会包含一些

标签，用来告诉浏览器关于文档的附加信息。在将来，创作者可能会利用预先定义好的标准文档的元数据配置文件（metadata profile），以便更好地描述它们的文档。profile 属性提供了与当前文档相关联的配置文件的 URL。

配置文件的格式以及浏览器使用它们的方式都还没有进行定义，这个属性主要是为将来的开发而保留的占位符。

‍

# body标签
body 元素定义文档的主体。

body 元素包含文档的所有内容（比如文本、超链接、图像、表格和列表等等。）


> HTML 与 XHTML 之间的差异
> 	1. 在 HTML 4.01 中，所有 body 元素的“呈现属性”均不被赞成使用。
> 	2. 在 XHTML 1.0 Strict DTD 中，所有 body 元素的“呈现属性”均不被支持。



## 可选的属性
| 属性 | 值 | 描述 | 
|---|---|---|
| alink | 颜色代码 | 不赞成使用。请使用样式取代它。规定文档中活动链接（active link）的的颜色。 |

| background | URL | 不赞成使用。请使用样式取代它。规定文档的背景图像。 |
。。。。



 	 	