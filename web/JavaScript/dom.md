---
title: DOM操作
description: 
published: true
date: 2023-03-26T08:05:41.200Z
tags: 
editor: markdown
dateCreated: 2023-02-25T11:33:47.548Z
---

# DOM
## 介绍
文档对象模型（Document Object Model，简称DOM），是 [W3C](https://baike.baidu.com/item/W3C) 组织推荐的处理[可扩展标记语言](https://baike.baidu.com/item/%E5%8F%AF%E6%89%A9%E5%B1%95%E7%BD%AE%E6%A0%87%E8%AF%AD%E8%A8%80)（html或者xhtml）的标准[编程接口](https://baike.baidu.com/item/%E7%BC%96%E7%A8%8B%E6%8E%A5%E5%8F%A3)。

W3C 已经定义了一系列的 DOM 接口，通过这些 DOM 接口可以改变网页的内容、结构和样式。

> DOM是W3C组织制定的一套处理 html和xml文档的规范，所有的浏览器都遵循了这套标准。

### DOM树

![https://cloutp.github.io/wiki/web/JavaScript/11DOM/web/JavaScript/images/DOM.png](https://cloutp.github.io/wiki/web/JavaScript/11DOM/web/JavaScript/images/DOM.png)

DOM树 又称为文档树模型，把文档映射成树形结构，通过节点对象对其处理，处理的结果可以加入到当前的页面。

* 文档：一个页面就是一个文档，DOM中使用document表示
* 节点：网页中的所有内容，在文档树中都是节点（标签、属性、文本、注释等），使用node表示
* 标签节点：网页中的所有标签，通常称为元素节点，又简称为“元素”，使用element表示

> DOM把以上内容都看作是对象

## 详细操作
- [获取元素](/JavaScript/dom/获取元素)
- [操作元素](/JavaScript/dom/操作元素)
- [自定义属性](/JavaScript/dom/自定义属性)
- [节点操作](/JavaScript/dom/节点操作)
- [classList](/JavaScript/dom/classList)


