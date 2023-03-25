---
title: random模块
description: 
published: true
date: 2023-03-06T05:32:32.162Z
tags: 
editor: markdown
dateCreated: 2023-02-26T08:10:21.198Z
---

# random模块

`import random

# 大于0且小于1之间的小数

print(random.random()) 0.42866657593385415

# 大于等于1且小于等于3之间的整数

print(random.randint(1, 3)) 3

# 大于等于1且小于3之间的整数

print(random.randrange(1, 3)) 2

# 大于1小于3的小数，如1.927109612082716

print(random.uniform(1, 3)) 2.1789596280319605

# 列表内的任意一个元素，即1或者‘23’或者[4,5]

print(random.choice([1, '23', [4, 5]])) [4, 5]

# random.sample([], n)，列表元素任意n个元素的组合，示例n=2

print(random.sample([1, '23', [4, 5]], 2)) ['23', 1] lis = [1, 3, 5, 7, 9]

# 打乱l的顺序,相当于"洗牌"

random.shuffle(lis) print(lis) [9, 1, 5, 7, 3]`