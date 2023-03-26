---
title: 函数
description: 
published: true
date: 2023-03-26T08:04:51.582Z
tags: 
editor: markdown
dateCreated: 2023-02-26T04:35:23.597Z
---

# 函数
在编程过程中对于使用频率较高的代码块，我们可以进行一次打包，在需要调用的地方直接调用，打包的过程就是函数的定义
函数的使用必须遵循’先定义，后调用’的原则。函数的定义就相当于事先将函数体代码保存起来，然后将内存地址赋值给函数名，函数名就是对这段代码的引用，这和变量的定义是相似的。没有事先定义函数而直接调用，就相当于在引用一个不存在的’变量名’。

## 定义函数

```py
def 函数名(参数1,参数2,...):     
	"""文档描述"""     
  函数体     
  return 值
```

1. def: 定义函数的关键字；
2. 函数名：函数名指向函数内存地址，是对函数体代码的引用。函数的命名应该反映出函数的功能；
3. 括号：括号内定义参数，参数是可有可无的，且无需指定参数的类型；
4. 冒号：括号后要加冒号，然后在下一行开始缩进编写函数体的代码；
5. """文档描述""": 描述函数功能，参数介绍等信息的文档，非必要，但是建议加上，从而增强函数的可读性；
6. 函数体：由语句和表达式组成；
7. return 值：定义函数的返回值，return是可有可无的。

参数是函数的调用者向函数体传值的媒介，若函数体代码逻辑依赖外部传来的参数时则需要定义为带参函数，

```py
def my_min(x,y):     
	res=x 
  if x < y 
  else y     
  return res
```

否则定义为无参函数

```py
def interactive():     
	user=input('user>>: ').strip()     
  pwd=input('password>>: ').strip()     
  return (user,pwd)
```

函数体为pass代表什么都不做，称之为空函数。定义空函数通常是有用的，因为在程序设计的开始，往往是先想好程序都需要完成什么功能，然后把所有功能都列举出来用pass充当函数体“占位符”，这将使得程序的体系结构立见，清晰且可读性强。例如要编写一个ftp程序，我们可能想到的功能有用户认证，下载，上传，浏览，切换目录等功能，可以先做出如下定义：

```py
def auth_user():     
	"""user authentication function"""     
  pass 
  
def download_file():     
	"""download file function"""     
  pass 
  
def upload_file():     
	"""upload file function"""     
	pass 
  
def ls():     
	"""list contents function"""     
  pass 
  
def cd():     
	"""change directory"""     
  pass
```

之后我们便可以统筹安排编程任务，有选择性的去实现上述功能来替换掉pass，从而提高开发效率。

# 调用函数与函数返回值

函数的使用分为定义阶段与调用阶段，定义函数时只检测语法，不执行函数体代码，函数名加括号即函数调用，只有调用函数时才会执行函数体代码

```py
#定义阶段 
def foo():     
	print('in the foo')     
  bar() 
  
def bar():     
print('in the bar') 
#调用阶段 
foo()
```

执行结果：

`in the foo in the bar`

定义阶段函数foo与bar均无语法错误，而在调用阶段调用foo()时，函数foo与bar都早已经存在于内存中了，所以不会有任何问题。

按照在程序出现的形式和位置，可将函数的调用形式分为三种：

```py
#1、语句形式： 
foo() 
#2、表达式形式： 
m=my_min(1,2) 
#将调用函数的返回值赋值给x 
n=10*my_min(1,2) #将调用函数的返回值乘以10的结果赋值给n 
#3、函数调用作为参数的形式：
# my_min（2，3）作为函数my_min的第二个参数，实现了取1,2,3中的较小者赋值给m
m=my_min(1，my_min（2，3）)
```

若需要将函数体代码执行的结果返回给调用者，则需要用到return。return后无值或直接省略return，则默认返回None，return的返回值无类型限制，且可以将多个返回值放到一个元组内。

```py
def test(x,y,z): 
	...     
  return x,y,z #等同于return (x,y,z) ...

res=test(1,2,3) print(res) (1, 2, 3)
```

return是一个函数结束的标志,函数内可以有多个return，但只执行一次函数就结束了，并把return后定义的值作为本次调用的结果返回。

# 详细
- [函数参数](/backend/python/function/函数参数)
- [闭包](/backend/python/function/闭包)
- [装饰器](/backend/python/function/装饰器)
- [迭代器](/backend/python/function/迭代器)
- [列表推导式](/backend/python/function/列表推导式)
- [生成器](/backend/python/function/生成器)
- [匿名函数](/backend/python/function/匿名函数)
- [作用域](/backend/python/function/作用域)
- [命名空间](/backend/python/function/命名空间)






