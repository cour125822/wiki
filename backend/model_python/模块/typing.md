---
title: typing
description: 
published: true
date: 2023-03-06T05:32:06.841Z
tags: 
editor: markdown
dateCreated: 2023-02-26T08:11:04.304Z
---

# 引言

* 前言：很多人在写完代码一段时间后回过头看代码，很可能忘记了自己写的函数需要传什么参数，返回什么类型的结果，就不得不去阅读代码的具体内容，降低了阅读的速度，加上Python本身就是一门弱类型的语言，这种现象就变得更加的严重，而typing这个模块很好的解决了这个问题。 系列文章

# typing模块的作用

1. 类型检查，防止运行时出现参数和返回值类型不符合。
2. 作为开发文档附加说明，方便使用者调用时传入和返回参数类型。
3. 该模块加入后并不会影响程序的运行，不会报正式的错误，只有提醒。
4. 注意：typing模块只有在python3.5以上的版本中才可以使用,pycharm目前支持typing检查

# 使用typing模块

`from typing import List, Tuple, Dict def add(a: int, string: str, f: float,         b: bool) -> Tuple[List, Tuple, Dict, bool]:     list1 = list(range(a))     tup = (string, string, string)     d = {"a": f}     bl = b     return list1, tup, d, bl print(add(5, "hhhh", 2.3, False)) ([0, 1, 2, 3, 4], ('hhhh', 'hhhh', 'hhhh'), {'a': 2.3}, False)`

* 在传入参数时通过”参数名:类型”的形式声明参数的类型；
* 返回结果通过”-> 结果类型”的形式声明结果的类型。
* 在调用的时候如果参数的类型不正确pycharm会有提醒，但不会影响程序的运行。
* 对于如list列表等，还可以规定得更加具体一些，如：”-> List[str]”,规定返回的是列表，并且元素是字符串。

`from typing import List def func(a: int, string: str) -> List[int or str]:  # 使用or关键字表示多种类型     list1 = []     list1.append(a)     list1.append(string)     return list1`

# typing常用类型

* int、long、float: 整型、长整形、浮点型
* bool、str: 布尔型、字符串类型
* List、 Tuple、 Dict、 Set:列表、元组、字典、集合
* Iterable、Iterator:可迭代类型、迭代器类型
* Generator：生成器类型