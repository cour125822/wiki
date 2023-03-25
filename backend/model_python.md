---
title: 模块
description: 
published: true
date: 2023-03-07T01:56:25.773Z
tags: 
editor: markdown
dateCreated: 2023-02-26T08:01:03.447Z
---

# 模块
> 在Python中，一个py文件就是一个模块，文件名为xxx.py模块名则是xxx。将程序模块化会使得程序的组织结构清晰，维护起来更加方便。比起直接开发一个完整的程序，单独开发一个小的模块也会更加简单，并且程序中的模块与电脑中的零部件稍微不同的是：程序中的模块可以被重复使用。所以总结下来，使用模块既保证了代码的重用性，又增强了程序的结构性和可维护性。另外除了自定义模块外，我们还可以导入使用内置或第三方模块提供的现成功能，

## 编写一个规范的模块

我们在编写py文件时，需要时刻提醒自己，该文件既是给自己用的，也有可能会被其他人使用，因而代码的可读性与易维护性显得十分重要，为此我们在编写一个模块时最好按照统一的规范去编写，如下

```python
#!/usr/bin/env python #通常只在类unix环境有效,作用是可以使用脚本名来执行，而无需直接调用解释器。 
"The module is used to..." #模块的文档描述 
import sys 	#导入模块 
x=1		#定义全局变量,如果非必须,则最好使用局部变量,这样可以提高代码的易维护性,并且可以节省内存提高性能 
class Foo:	#定义类,并写好类的注释     
	"""Class Foo is used to..."""  
	pass def test(): #定义函数,并写好函数的注释     
  	'Function test is used to…'     
pass if __name__ == '__main__': #主程序     
	test() #在被当做脚本执行时,执行此处的代码
```


[模块导入](/backend/model_python/模块导入)
‍[文件分类](/backend/model_python/文件分类)



# 包

随着模块数目的增多，把所有模块不加区分地放到一起也是极不合理的，于是Python为我们提供了一种把模块组织到一起的方法，即创建一个包。包就是一个含有__init__.py文件的文件夹，文件夹内可以组织子模块或子包，例如

```shell
pool/                #顶级包 
├── __init__.py      
├── futures          #子包 
│   ├── __init__.py 
│   ├── process.py 
│   └── thread.py 
└── versions.py      #子模块
```

需要强调的是

```
#1. 在python3中，即使包下没有__init__.py文件，import 包仍然不会报错，而在python2中，包下一定要有该文件，否则import 包报错 
#2. 创建包的目的不是为了运行，而是被导入使用，记住，包只是模块的一种形式而已，包的本质就是一种模块
```

接下来我们就以包pool为例来介绍包的使用，包内各文件内容如下

```python
# [process.py]
class ProcessPoolExecutor: 
	def __init__(self,max_workers): 
  self.max_workers=max_workers 
  def submit(self): 
  	print('ProcessPool submit')

# [thread.py]
class ThreadPoolExecutor: 
	def **init**(self, max_workers): 
  	self.max_workers = max_workers 
    def submit(self): 
    	print('ThreadPool submit')

# [versions.py]
def check(): 
	print('check versions’)

# __init__.py文件内容均为空
```
[导入包](/backend/model_python/导入包)

# 常用模块
[模块](/backend/model_python/模块)