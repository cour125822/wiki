---
title: CSS选择器
description: 
published: true
date: 2023-03-06T03:05:52.281Z
tags: 
editor: markdown
dateCreated: 2023-02-25T10:54:42.090Z
---

**CSS 选择器**规定了 CSS 规则会被应用到哪些元素上。

>  **备注：** 暂时没有能够选择 父元素、父元素的同级元素，或 父元素的同级元素的子元素 的选择器或者组合器。

## 基本选择器

1. [通用选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Universal_selectors)：选择所有元素。（可选）可以将其限制为特定的名称空间或所有名称空间。 **语法：**​`*` `ns|*` `*|*` **例子：**​`*` 将匹配文档的所有元素。
2. [元素选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Type_selectors)：按照给定的节点名称，选择所有匹配的元素。 **语法：**​`elementname` **例子：**​`input` 匹配任何 [&lt;input&gt;](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Input)`` 元素。
3. [类选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Class_selectors)（[Class selector](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Class_selectors)）按照给定的 `class` 属性的值，选择所有匹配的元素。 **语法**：`.classname` **例子**：`.index` 匹配任何 `class` 属性中含有 "index" 类的元素。
4. [ID 选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/ID_selectors)（[ID selector](https://developer.mozilla.org/zh-CN/docs/Web/CSS/ID_selectors)）按照 `id` 属性选择一个与之匹配的元素。需要注意的是，一个文档中，每个 ID 属性都应当是唯一的。 **语法：**​`#idname` **例子：**​`#toc` 匹配 ID 为 "toc" 的元素。
5. [属性选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Attribute_selectors)（[Attribute selector](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Attribute_selectors)）按照给定的属性，选择所有匹配的元素。 **语法：**​`[attr]` `[attr=value]` `[attr~=value]` `[attr|=value]` `[attr^=value]` `[attr$=value]` `[attr*=value]` **例子：**​`[autoplay]` 选择所有具有 `autoplay` 属性的元素（不论这个属性的值是什么）。

## 分组选择器

1. [选择器列表](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Selector_list)（[Selector list](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Selector_list)）`,` 是将不同的选择器组合在一起的方法，它选择所有能被列表中的任意一个选择器选中的节点。 **语法**：`A, B` **示例**：`div, span` 会同时匹配 [&lt;span&gt;](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/span)`` 元素和 [&lt;div&gt;](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/div)`` 元素。

## 组合器（Combinators）

1. [后代组合器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Descendant_combinator)（[Descendant combinator](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Descendant_combinator)）“ ”（空格）组合器选择前一个元素的后代节点。 **语法：**​`A B` **例子：**​`div span` 匹配所有位于任意 [&lt;div&gt;](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/div)`` 元素之内的 [&lt;span&gt;](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/span)`` 元素。
2. [直接子代组合器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Child_combinator)（[Child combinator](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Child_combinator)）`>` 组合器选择前一个元素的直接子代的节点。 **语法**：`A > B` **例子**：`ul > li` 匹配直接嵌套在 [&lt;ul&gt;](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/ul)`` 元素内的所有 [&lt;li&gt;](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/li)`` 元素。
3. [一般兄弟组合器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/General_sibling_combinator)（[General sibling combinator](https://developer.mozilla.org/zh-CN/docs/Web/CSS/General_sibling_combinator)）`~` 组合器选择兄弟元素，也就是说，后一个节点在前一个节点后面的任意位置，并且共享同一个父节点。 **语法**：`A ~ B` **例子**：`p ~ span` 匹配同一父元素下，[&lt;p&gt;](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/p)`` 元素后的所有 [&lt;span&gt;](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/span)`` 元素。
4. [紧邻兄弟组合器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Adjacent_sibling_combinator)（[Adjacent sibling combinator](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Adjacent_sibling_combinator)）`+` 组合器选择相邻元素，即后一个元素紧跟在前一个之后，并且共享同一个父节点。 **语法：**​`A + B` **例子：**​`h2 + p` 会匹配所有紧邻在 [&lt;h2&gt;](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements "Currently only available in English (US)")``​[ (en-US)](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements "Currently only available in English (US)") 元素后的 [&lt;p&gt;](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/p)`` 元素。
5. [列组合器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Column_combinator)（[Column combinator](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Column_combinator)）**实验性**​`||` 组合器选择属于某个表格行的节点。 **语法：** `A || B` **例子：** `col || td` 会匹配所有 [&lt;col&gt;](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/col)`` 作用域内的 [&lt;td&gt;](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/td)`` 元素。

## 伪选择器（Pseudo）

1. [伪类](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-classes)​`:` 伪选择器支持按照未被包含在文档树中的状态信息来选择元素。 **例子：**​`a:visited` 匹配所有曾被访问过的 [&lt;a&gt;](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a)`` 元素。
2. [伪元素](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-elements)​`::` 伪选择器用于表示无法用 HTML 语义表达的实体。 **例子：**​`p::first-line` 匹配所有 [&lt;p&gt;](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/p)`` 元素的第一行。



## CSS优先级![image](assets/image-20230204164336-ihvc779.png)


![image](assets/image-20230204164301-8m3h2jb.png)


## CSS效率

| 选择器       | 用法                  |
| -------------- | ----------------------- |
| id选择器     | #myid                 |
| 类选择器     | .myclassname          |
| 标签选择器   | div,h1,p              |
| 相邻选择器   | h1+p                  |
| 子选择器     | ul > li               |
| 后代选择器   | li a                  |
| 通配符选择器 | *                     |
| 属性选择器   | a[rel="external"]     |
| 伪类选择器   | a:hover, li:nth-child |