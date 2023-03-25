---
title: __方法
description: 
published: true
date: 2023-03-07T01:53:29.812Z
tags: 
editor: markdown
dateCreated: 2023-02-26T06:33:19.792Z
---

# isinstance、issubclass
## isinstance与type

在游戏项目中，我们会在每个接口验证客户端传过来的参数类型，如果验证不通过，返回给客户端“参数错误”错误码。

这样做不但便于调试，而且增加健壮性。因为客户端是可以作弊的，不要轻易相信客户端传过来的参数。

验证类型用type函数，非常好用，比如

```
print(type('foo') == str)
True
print(type(2.3) in (int, float))
True
```

既然有了type()来判断类型，为什么还有isinstance()呢？

一个明显的区别是在判断子类。

type()不会认为子类是一种父类类型；isinstance()会认为子类是一种父类类型。

千言不如一码。

```
class Foo(object):
    pass
 
class Bar(Foo):
    pass
 
print(type(Foo()) == Foo)
True
print(type(Bar()) == Foo)
False
# isinstance参数为对象和类
print(isinstance(Bar(),Foo))
True
```

需要注意的是，旧式类跟新式类的type()结果是不一样的。旧式类都是<type ‘instance’>。

```
# python2.+
class A:
    pass
 
class B:
    pass
 
class C(object):
    pass

print('old style class',type(A()))  # old style class <type 'instance'>

print('old style class',type(B()))  # old style class <type 'instance'>

print('new style class',type(C()))  # new style class <class '__main__.C'>

print(type(A()) == type(B()))  # True
```

注意：**不存在说isinstance比type更好。只有哪个更适合需求。**

## issubclass

```
class Parent:
    pass


class Sub(Parent):
    pass


print(issubclass(Sub, Parent))
True
print(issubclass(Parent, object))
True
```


# 反射(hasattr和getattr和setattr和delattr)
## 反射在类中的使用

反射就是通过字符串来操作类或者对象的属性

- 反射本质就是在使用内置函数，其中反射有以下四个内置函数：

```
1. hasattr：判断一个方法是否存在与这个类中
2. getattr：根据字符串去获取obj对象里的对应的方法的内存地址，加"()"括号即可执行
3. setattr：通过setattr将外部的一个函数绑定到实例中
4. delattr：删除一个实例或者类中的方法
class People:
    country = 'China'

    def __init__(self, name):
        self.name = name

    def eat(self):
        print('%s is eating' % self.name)


peo1 = People('lqz')
print(hasattr(peo1, 'eat'))  # peo1.eat
True
print(getattr(peo1, 'eat'))  # peo1.eat
<bound method People.eat of <__main__.People object at 0x1043b9f98>>
print(getattr(peo1, 'xxxxx', None))
None
setattr(peo1, 'age', 18)  # peo1.age=18
print(peo1.age)
18
print(peo1.__dict__)
{'name': 'egon', 'age': 18}
delattr(peo1, 'name')  # del peo1.name
print(peo1.__dict__)
{'age': 18}
```

## 应用

需求：通过用户输入命令启动功能

```
class Ftp:
    def __init__(self, ip, port):
        self.ip = ip
        self.port = port

    def get(self):
        print('GET function')

    def put(self):
        print('PUT function')

    def run(self):
        while True:
            choice = input('>>>: ').strip()

            if choice == 'q':
                print('break')
                break


#             print(choice, type(choice))
#             if hasattr(self, choice):
#                 method = getattr(self, choice)
#                 method()
#             else:
#                 print('输入的命令不存在')

            method = getattr(self, choice, None)

            if method is None:
                print('输入的命令不存在')
            else:
                method()
conn = Ftp('1.1.1.1', 23)
conn.run()
>>>: time
输入的命令不存在
>>>: time
输入的命令不存在
>>>: q
break
```

## 反射在模块中的使用

###  前言

```
# test.py
def f1():
    print('f1')


def f2():
    print('f2')


def f3():
    print('f3')


def f4():
    print('f4')


a = 1
import test as ss

ss.f1()
ss.f2()
print(ss.a)
```

我们要导入另外一个模块,可以使用import.现在有这样的需求,我动态输入一个模块名，可以随时访问到导入模块中的方法或者变量，怎么做呢？

```
imp = input(“请输入你想导入的模块名:”)
CC = __import__(imp) 這种方式就是通过输入字符串导入你所想导入的模块 
CC.f1()  # 执行模块中的f1方法
```

上面我们实现了动态输入模块名，从而使我们能够输入模块名并且执行里面的函数。但是上面有一个缺点，那就是执行的函数被固定了。那么，我们能不能改进一下，动态输入函数名，并且来执行呢？

```
# dynamic.py
imp = input("请输入模块:")
dd = __import__(imp)
# 等价于import imp
inp_func = input("请输入要执行的函数：")

f = getattr(dd, inp_func,
            None)  # 作用:从导入模块中找到你需要调用的函数inp_func,然后返回一个该函数的引用.没有找到就烦会None

f()  # 执行该函数
请输入模块:time
请输入要执行的函数：time

1560959528.6127071
```

上面我们就实现了，动态导入一个模块，并且动态输入函数名然后执行相应功能。

当然，上面还存在一点点小问题:那就是我的模块名有可能不是在本级目录中存放着。有可能是如下图存放方式：

```
|- day24
    |- lib
        |- common.py
```

那么这种方式我们该如何搞定呢?看下面代码:

```
dd = __import__("lib.text.commons")  # 这样仅仅导入了lib模块
dd = __import__("lib.text.commons", fromlist=True)  # 改用这种方式就能导入成功
# 等价于import config
inp_func = input("请输入要执行的函数：")
f = getattr(dd, inp_func)
f()
```

### 反射机制

上面说了那么多，到底什么是反射机制呢?

其实，反射就是通过字符串的形式，导入模块；通过字符串的形式，去模块寻找指定函数，并执行。利用字符串的形式去对象（模块）中操作（查找/获取/删除/添加）成员，一种基于字符串的事件驱动！

先来介绍四个内置函数:

#### getattr()

getattr()函数是Python自省的核心函数，具体使用大体如下：

```
class A:
    def __init__(self):
        self.name = 'lqz'
        # self.age='18'

    def method(self):
        print("method print")


a = A()

print(getattr(a, 'name',
              'not find'))  # 如果a 对象中有属性name则打印self.name的值，否则打印'not find'
print(getattr(a, 'age',
              'not find'))  # 如果a 对象中有属性age则打印self.age的值，否则打印'not find'
print(getattr(a, 'method', 'default'))  # 如果有方法method，否则打印其地址，否则打印default
print(getattr(a, 'method', 'default')())  # 如果有方法method，运行函数并打印None否则打印default
```

#### hasattr(object, name)

说明：判断对象object是否包含名为name的特性（hasattr是通过调用getattr(ojbect, name)是否抛出异常来实现的）

#### setattr(object, name, value)

这是相对应的getattr()。参数是一个对象,一个字符串和一个任意值。字符串可能会列出一个现有的属性或一个新的属性。这个函数将值赋给属性的。该对象允许它提供。例如,setattr(x,“foobar”,123)相当于x.foobar = 123。

#### delattr(object, name)

与setattr()相关的一组函数。参数是由一个对象(记住python中一切皆是对象)和一个字符串组成的。string参数必须是对象属性名之一。该函数删除该obj的一个由string指定的属性。delattr(x, ‘foobar’)=del x.foobar

我们可以利用上述的四个函数,来对模块进行一系列操作.

```
r = hasattr(commons, xxx)  # 判断某个函数或者变量是否存在
print(r)

setattr(commons, 'age', 18)  # 给commons模块增加一个全局变量age = 18，创建成功返回none

setattr(commons, 'age', lambda a: a + 1)  # 给模块添加一个函数

delattr(commons, 'age')  # 删除模块中某个变量或者函数
```

注意：**getattr,hasattr,setattr,delattr对模块的修改都在内存中进行，并不会影响文件中真实内容。**

### 应用

基于反射机制模拟web框架路由

需求：比如我们输入<[www.xxx.com/commons/f1>](http://www.xxx.com/commons/f1>) ，返回f1的结果。

```
# 动态导入模块，并执行其中函数
url = input("url: ")

target_module, target_func = url.split('/')
m = __import__('lib.' + target_module, fromlist=True)

inp = url.split("/")[-1]  # 分割url,并取出url最后一个字符串
if hasattr(m, target_func):  # 判断在commons模块中是否存在inp这个字符串
    target_func = getattr(m, target_func)  # 获取inp的引用
    target_func()  # 执行
else:
    print("404")
```
# __setattr__和__delattr__和__getattr__

```
class Foo:
    x = 1

    def __init__(self, y):
        self.y = y

    def __getattr__(self, item):
        print('----> from getattr:你找的属性不存在')

    def __setattr__(self, key, value):
        print('----> from setattr')
        # self.key = value  # 这就无限递归了,你好好想想
        # self.__dict__[key] = value  # 应该使用它

    def __delattr__(self, item):
        print('----> from delattr')
        # del self.item  # 无限递归了
        self.__dict__.pop(item)


f1 = Foo(10)
```

## \_\_setattr\_\_

- 添加/修改属性会触发它的执行

```
print(f1.__dict__)  
# 因为你重写了__setattr__，凡是赋值操作都会触发它的运行，你啥都没写，就是根本没赋值，除非你直接操作属性字典，否则永远无法赋值
f1.z = 3
print(f1.__dict__)
```

## \_\_delattr\_\_

- 删除属性的时候会触发

```
f1.__dict__['a'] = 3  # 我们可以直接修改属性字典，来完成添加/修改属性的操作
del f1.a
print(f1.__dict__)
----> from delattr
{}
```

## \_\_getattr\_\_

- 只有在使用点调用属性且属性不存在的时候才会触发

```
f1.xxxxxx
----> from getattr:你找的属性不存在
```
# __getattribute__
## **getattr**

- 不存在的属性访问，触发**getattr**

```
class Foo:
    def __init__(self, x):
        self.x = x

    def __getattr__(self, item):
        print('执行的是我')
        # return self.__dict__[item]


f1 = Foo(10)
print(f1.x)
10
f1.xxxxxx
执行的是我
```

## __getattribute__

- 查找属性无论是否存在，都会执行

```
class Foo:
    def __init__(self, x):
        self.x = x

    def __getattribute__(self, item):
        print('不管是否存在，我都会执行')


f1 = Foo(10)
f1.x
不管是否存在，我都会执行
f1.xxxxxx
不管是否存在，我都会执行
```

## __getattr__与__getattribute__

- 当**getattribute**与**getattr**同时存在，只会执行**getattrbute**，除非**getattribute**在执行过程中抛出异常AttributeError

```
#_*_coding:utf-8_*_
__author__ = 'Linhaifeng'


class Foo:
    def __init__(self, x):
        self.x = x

    def __getattr__(self, item):
        print('执行的是我')
        # return self.__dict__[item]
    def __getattribute__(self, item):
        print('不管是否存在,我都会执行')
        raise AttributeError('哈哈')


f1 = Foo(10)
f1.x
不管是否存在,我都会执行
执行的是我
f1.xxxxxx
不管是否存在,我都会执行
执行的是我
```


# 描述符(__get__和__set__和__delete__)

## 描述符

- 描述符是什么：描述符本质就是一个新式类，在这个新式类中，至少实现了get()，set()，delete()中的一个，这也被称为描述符协议

  - **get**()：调用一个属性时，触发
- **set**()：为一个属性赋值时，触发
  - **delete**()：采用del删除属性时，触发

- 定义一个描述符

```
class Foo:  # 在python3中Foo是新式类，它实现了__get__()，__set__()，__delete__()中的一个三种方法的一个，这个类就被称作一个描述符
    def __get__(self, instance, owner):
        pass

    def __set__(self, instance, value):
        pass

    def __delete__(self, instance):
        pass
```

## 描述符的作用

- 描述符是干什么的：描述符的作用是用来代理另外一个类的属性的，必须把描述符定义成这个类的类属性，不能定义到构造函数中

```
class Foo:
    def __get__(self, instance, owner):
        print('触发get')

    def __set__(self, instance, value):
        print('触发set')

    def __delete__(self, instance):
        print('触发delete')


f1 = Foo()
```

- 包含这三个方法的新式类称为描述符，由这个类产生的实例进行属性的调用/赋值/删除，并不会触发这三个方法

```
f1.name = 'lqz'
f1.name
del f1.name
```

### 何时，何地，会触发这三个方法的执行

```
class Str:
    """描述符Str"""

    def __get__(self, instance, owner):
        print('Str调用')

    def __set__(self, instance, value):
        print('Str设置...')

    def __delete__(self, instance):
        print('Str删除...')


class Int:
    """描述符Int"""

    def __get__(self, instance, owner):
        print('Int调用')

    def __set__(self, instance, value):
        print('Int设置...')

    def __delete__(self, instance):
        print('Int删除...')


class People:
    name = Str()
    age = Int()

    def __init__(self, name, age):  # name被Str类代理，age被Int类代理
        self.name = name
        self.age = age


# 何地？：定义成另外一个类的类属性

# 何时？：且看下列演示

p1 = People('alex', 18)
Str设置...
Int设置...
```

- 描述符Str的使用

```
p1.name
p1.name = 'lqz'
del p1.name
Str调用
Str设置...
Str删除...
```

- 描述符Int的使用

```
p1.age
p1.age = 18
del p1.age
Int调用
Int设置...
Int删除...
```

- 我们来瞅瞅到底发生了什么

```
print(p1.__dict__)
print(People.__dict__)
{}
{'__module__': '__main__', 'name': <__main__.Str object at 0x107a86940>, 'age': <__main__.Int object at 0x107a863c8>, '__init__': <function People.__init__ at 0x107ba2ae8>, '__dict__': <attribute '__dict__' of 'People' objects>, '__weakref__': <attribute '__weakref__' of 'People' objects>, '__doc__': None}
```

- 补充

```
print(type(p1) == People)  # type(obj)其实是查看obj是由哪个类实例化来的
print(type(p1).__dict__ == People.__dict__)
True
True
```

## 两种描述符

### 数据描述符

- 至少实现了**get**()和**set**()

```
class Foo:
    def __set__(self, instance, value):
        print('set')

    def __get__(self, instance, owner):
        print('get')
```

### 非数据描述符

- 没有实现**set**()

```
class Foo:
    def __get__(self, instance, owner):
        print('get')
```

## 描述符注意事项



1. 描述符本身应该定义成新式类，被代理的类也应该是新式类

2. 必须把描述符定义成这个类的类属性，不能为定义到构造函数中

3. 要严格遵循该优先级，优先级由高到底分别是

   1.类属性

   2.数据描述符

   3.实例属性

   4.非数据描述符

   5.找不到的属性触发**getattr**()

## 使用描述符

- 众所周知，python是弱类型语言，即参数的赋值没有类型限制，下面我们通过描述符机制来实现类型限制功能

### 牛刀小试

```
class Str:
    def __init__(self, name):
        self.name = name

    def __get__(self, instance, owner):
        print('get--->', instance, owner)
        return instance.__dict__[self.name]

    def __set__(self, instance, value):
        print('set--->', instance, value)
        instance.__dict__[self.name] = value

    def __delete__(self, instance):
        print('delete--->', instance)
        instance.__dict__.pop(self.name)


class People:
    name = Str('name')

    def __init__(self, name, age, salary):
        self.name = name
        self.age = age
        self.salary = salary


p1 = People('lqz', 18, 3231.3)
set---> <__main__.People object at 0x107a86198> lqz
```

- 调用

```
print(p1.__dict__)
{'name': 'lqz', 'age': 18, 'salary': 3231.3}
print(p1.name)
get---> <__main__.People object at 0x107a86198> <class '__main__.People'>
lqz
```

- 赋值

```
print(p1.__dict__)
{'name': 'lqz', 'age': 18, 'salary': 3231.3}
p1.name = 'lqzlin'
print(p1.__dict__)
set---> <__main__.People object at 0x107a86198> lqzlin
{'name': 'lqzlin', 'age': 18, 'salary': 3231.3}
```

- 删除

```
print(p1.__dict__)
{'name': 'lqzlin', 'age': 18, 'salary': 3231.3}
del p1.name
print(p1.__dict__)
delete---> <__main__.People object at 0x107a86198>
{'age': 18, 'salary': 3231.3}
```

### 拔刀相助

```
class Str:
    def __init__(self, name):
        self.name = name

    def __get__(self, instance, owner):
        print('get--->', instance, owner)
        return instance.__dict__[self.name]

    def __set__(self, instance, value):
        print('set--->', instance, value)
        instance.__dict__[self.name] = value

    def __delete__(self, instance):
        print('delete--->', instance)
        instance.__dict__.pop(self.name)


class People:
    name = Str('name')

    def __init__(self, name, age, salary):
        self.name = name
        self.age = age
        self.salary = salary


# 疑问：如果我用类名去操作属性呢
try:
    People.name  # 报错，错误的根源在于类去操作属性时，会把None传给instance
except Exception as e:
    print(e)
get---> None <class '__main__.People'>
'NoneType' object has no attribute '__dict__'
```

- 修订**get**方法

```
class Str:
    def __init__(self, name):
        self.name = name

    def __get__(self, instance, owner):
        print('get--->', instance, owner)
        if instance is None:
            return self
        return instance.__dict__[self.name]

    def __set__(self, instance, value):
        print('set--->', instance, value)
        instance.__dict__[self.name] = value

    def __delete__(self, instance):
        print('delete--->', instance)
        instance.__dict__.pop(self.name)


class People:
    name = Str('name')

    def __init__(self, name, age, salary):
        self.name = name
        self.age = age
        self.salary = salary


print(People.name)  # 完美，解决
get---> None <class '__main__.People'>
<__main__.Str object at 0x107a86da0>
```

### 磨刀霍霍

```
class Str:
    def __init__(self, name, expected_type):
        self.name = name
        self.expected_type = expected_type

    def __get__(self, instance, owner):
        print('get--->', instance, owner)
        if instance is None:
            return self
        return instance.__dict__[self.name]

    def __set__(self, instance, value):
        print('set--->', instance, value)
        if not isinstance(value, self.expected_type):  # 如果不是期望的类型，则抛出异常
            raise TypeError('Expected %s' % str(self.expected_type))
        instance.__dict__[self.name] = value

    def __delete__(self, instance):
        print('delete--->', instance)
        instance.__dict__.pop(self.name)


class People:
    name = Str('name', str)  # 新增类型限制str

    def __init__(self, name, age, salary):
        self.name = name
        self.age = age
        self.salary = salary


try:
    p1 = People(123, 18, 3333.3)  # 传入的name因不是字符串类型而抛出异常
except Exception as e:
    print(e)
set---> <__main__.People object at 0x1084cd940> 123
Expected <class 'str'>
```

### 大刀阔斧

```
class Typed:
    def __init__(self, name, expected_type):
        self.name = name
        self.expected_type = expected_type

    def __get__(self, instance, owner):
        print('get--->', instance, owner)
        if instance is None:
            return self
        return instance.__dict__[self.name]

    def __set__(self, instance, value):
        print('set--->', instance, value)
        if not isinstance(value, self.expected_type):
            raise TypeError('Expected %s' % str(self.expected_type))
        instance.__dict__[self.name] = value

    def __delete__(self, instance):
        print('delete--->', instance)
        instance.__dict__.pop(self.name)


class People:
    name = Typed('name', str)
    age = Typed('name', int)
    salary = Typed('name', float)

    def __init__(self, name, age, salary):
        self.name = name
        self.age = age
        self.salary = salary


try:
    p1 = People(123, 18, 3333.3)
except Exception as e:
    print(e)
set---> <__main__.People object at 0x1082c7908> 123
Expected <class 'str'>
try:
    p1 = People('lqz', '18', 3333.3)
except Exception as e:
    print(e)
set---> <__main__.People object at 0x1078dd438> lqz
set---> <__main__.People object at 0x1078dd438> 18
Expected <class 'int'>
p1 = People('lqz', 18, 3333.3)
set---> <__main__.People object at 0x1081b3da0> lqz
set---> <__main__.People object at 0x1081b3da0> 18
set---> <__main__.People object at 0x1081b3da0> 3333.3
```

- 大刀阔斧之后我们已然能实现功能了，但是问题是，如果我们的类有很多属性，你仍然采用在定义一堆类属性的方式去实现，low，这时候我需要教你一招：独孤九剑

#### 类的装饰器:无参

```
def decorate(cls):
    print('类的装饰器开始运行啦------>')
    return cls


@decorate  # 无参：People = decorate(People)
class People:
    def __init__(self, name, age, salary):
        self.name = name
        self.age = age
        self.salary = salary


p1 = People('lqz', 18, 3333.3)
类的装饰器开始运行啦------>
```

#### 类的装饰器:有参

```
def typeassert(**kwargs):
    def decorate(cls):
        print('类的装饰器开始运行啦------>', kwargs)
        return cls

    return decorate


@typeassert(
    name=str, age=int, salary=float
)  # 有参：1.运行typeassert(...)返回结果是decorate，此时参数都传给kwargs 2.People=decorate(People)
class People:
    def __init__(self, name, age, salary):
        self.name = name
        self.age = age
        self.salary = salary


p1 = People('lqz', 18, 3333.3)
类的装饰器开始运行啦------> {'name': <class 'str'>, 'age': <class 'int'>, 'salary': <class 'float'>}
```

### 刀光剑影

```
class Typed:
    def __init__(self, name, expected_type):
        self.name = name
        self.expected_type = expected_type

    def __get__(self, instance, owner):
        print('get--->', instance, owner)
        if instance is None:
            return self
        return instance.__dict__[self.name]

    def __set__(self, instance, value):
        print('set--->', instance, value)
        if not isinstance(value, self.expected_type):
            raise TypeError('Expected %s' % str(self.expected_type))
        instance.__dict__[self.name] = value

    def __delete__(self, instance):
        print('delete--->', instance)
        instance.__dict__.pop(self.name)


def typeassert(**kwargs):
    def decorate(cls):
        print('类的装饰器开始运行啦------>', kwargs)
        for name, expected_type in kwargs.items():
            setattr(cls, name, Typed(name, expected_type))
        return cls

    return decorate


@typeassert(
    name=str, age=int, salary=float
)  # 有参：1.运行typeassert(...)返回结果是decorate，此时参数都传给kwargs 2.People=decorate(People)
class People:
    def __init__(self, name, age, salary):
        self.name = name
        self.age = age
        self.salary = salary


print(People.__dict__)
p1 = People('lqz', 18, 3333.3)
类的装饰器开始运行啦------> {'name': <class 'str'>, 'age': <class 'int'>, 'salary': <class 'float'>}
{'__module__': '__main__', '__init__': <function People.__init__ at 0x10797a400>, '__dict__': <attribute '__dict__' of 'People' objects>, '__weakref__': <attribute '__weakref__' of 'People' objects>, '__doc__': None, 'name': <__main__.Typed object at 0x1080b2a58>, 'age': <__main__.Typed object at 0x1080b2ef0>, 'salary': <__main__.Typed object at 0x1080b2c18>}
set---> <__main__.People object at 0x1080b22e8> lqz
set---> <__main__.People object at 0x1080b22e8> 18
set---> <__main__.People object at 0x1080b22e8> 3333.3
```

## 描述符总结

- 描述符是可以实现大部分python类特性中的底层魔法，包括@classmethod，@staticmethd，@property甚至是**slots**属性
- 描述父是很多高级库和框架的重要工具之一，描述符通常是使用到装饰器或者元类的大型框架中的一个组件.

## 自定制@property

- 利用描述符原理完成一个自定制@property，实现延迟计算（本质就是把一个函数属性利用装饰器原理做成一个描述符：类的属性字典中函数名为key，value为描述符类产生的对象）

### property回顾

```
class Room:
    def __init__(self, name, width, length):
        self.name = name
        self.width = width
        self.length = length

    @property
    def area(self):
        return self.width * self.length


r1 = Room('alex', 1, 1)
print(r1.area)
1
```

### 自定制property

```
class Lazyproperty:
    def __init__(self, func):
        self.func = func

    def __get__(self, instance, owner):
        print('这是我们自己定制的静态属性，r1.area实际是要执行r1.area()')
        if instance is None:
            return self
        return self.func(instance)  # 此时你应该明白，到底是谁在为你做自动传递self的事情


class Room:
    def __init__(self, name, width, length):
        self.name = name
        self.width = width
        self.length = length

    @Lazyproperty  # area=Lazyproperty(area) 相当于定义了一个类属性,即描述符
    def area(self):
        return self.width * self.length


r1 = Room('alex', 1, 1)
print(r1.area)
这是我们自己定制的静态属性，r1.area实际是要执行r1.area()
1
```

### 实现延迟计算功能

```
class Lazyproperty:
    def __init__(self, func):
        self.func = func

    def __get__(self, instance, owner):
        print('这是我们自己定制的静态属性，r1.area实际是要执行r1.area()')
        if instance is None:
            return self
        else:
            print('--->')
            value = self.func(instance)
            setattr(instance, self.func.__name__, value)  # 计算一次就缓存到实例的属性字典中
            return value


class Room:
    def __init__(self, name, width, length):
        self.name = name
        self.width = width
        self.length = length

    @Lazyproperty  # area=Lazyproperty(area) 相当于'定义了一个类属性,即描述符'
    def area(self):
        return self.width * self.length


r1 = Room('alex', 1, 1)
print(r1.area)  # 先从自己的属性字典找,没有再去类的中找,然后出发了area的__get__方法
这是我们自己定制的静态属性，r1.area实际是要执行r1.area()
--->
1
print(r1.area)  # 先从自己的属性字典找,找到了,是上次计算的结果,这样就不用每执行一次都去计算
1
```

## 打破延迟计算

- 一个小的改动，延迟计算的美梦就破碎了

```
class Lazyproperty:
    def __init__(self, func):
        self.func = func

    def __get__(self, instance, owner):
        print('这是我们自己定制的静态属性，r1.area实际是要执行r1.area()')
        if instance is None:
            return self
        else:
            value = self.func(instance)
            instance.__dict__[self.func.__name__] = value
            return value
        # return self.func(instance) # 此时你应该明白,到底是谁在为你做自动传递self的事情
    def __set__(self, instance, value):
        print('hahahahahah')


class Room:
    def __init__(self, name, width, length):
        self.name = name
        self.width = width
        self.length = length

    @Lazyproperty  # area=Lazyproperty(area) 相当于定义了一个类属性,即描述符
    def area(self):
        return self.width * self.length
print(Room.__dict__)
{'__module__': '__main__', '__init__': <function Room.__init__ at 0x107d53620>, 'area': <__main__.Lazyproperty object at 0x107ba3860>, '__dict__': <attribute '__dict__' of 'Room' objects>, '__weakref__': <attribute '__weakref__' of 'Room' objects>, '__doc__': None}
r1 = Room('alex', 1, 1)
print(r1.area)
print(r1.area)
print(r1.area)
这是我们自己定制的静态属性，r1.area实际是要执行r1.area()
1
这是我们自己定制的静态属性，r1.area实际是要执行r1.area()
1
这是我们自己定制的静态属性，r1.area实际是要执行r1.area()
1
print(
    r1.area
)  #缓存功能失效,每次都去找描述符了,为何,因为描述符实现了set方法,它由非数据描述符变成了数据描述符,数据描述符比实例属性有更高的优先级,因而所有的属性操作都去找描述符了
这是我们自己定制的静态属性，r1.area实际是要执行r1.area()
1
```

## 自定制@classmethod

```
class ClassMethod:
    def __init__(self, func):
        self.func = func

    def __get__(
            self, instance,
            owner):  #类来调用,instance为None,owner为类本身,实例来调用,instance为实例,owner为类本身,
        def feedback():
            print('在这里可以加功能啊...')
            return self.func(owner)

        return feedback


class People:
    name = 'lqz'

    @ClassMethod  # say_hi=ClassMethod(say_hi)
    def say_hi(cls):
        print('你好啊,帅哥 %s' % cls.name)


People.say_hi()

p1 = People()
在这里可以加功能啊...
你好啊,帅哥 lqz
p1.say_hi()
在这里可以加功能啊...
你好啊,帅哥 lqz
```

- 疑问,类方法如果有参数呢,好说,好说

```
class ClassMethod:
    def __init__(self, func):
        self.func = func

    def __get__(self, instance, owner
                ):  # 类来调用,instance为None,owner为类本身,实例来调用,instance为实例,owner为类本身,
        def feedback(*args, **kwargs):
            print('在这里可以加功能啊...')
            return self.func(owner, *args, **kwargs)

        return feedback


class People:
    name = 'lqz'

    @ClassMethod  # say_hi=ClassMethod(say_hi)
    def say_hi(cls, msg):
        print('你好啊,帅哥 %s %s' % (cls.name, msg))


People.say_hi('你是那偷心的贼')

p1 = People()
在这里可以加功能啊...
你好啊,帅哥 lqz 你是那偷心的贼
p1.say_hi('你是那偷心的贼')
在这里可以加功能啊...
你好啊,帅哥 lqz 你是那偷心的贼
```

## 自定制@staticmethod

```
class StaticMethod:
    def __init__(self, func):
        self.func = func

    def __get__(
            self, instance,
            owner):  # 类来调用，instance为None，owner为类本身，实例来调用，instance为实例，owner为类本身
        def feedback(*args, **kwargs):
            print('在这里可以加功能啊...')
            return self.func(*args, **kwargs)

        return feedback


class People:
    @StaticMethod  # say_hi = StaticMethod(say_hi)
    def say_hi(x, y, z):
        print('------>', x, y, z)


People.say_hi(1, 2, 3)

p1 = People()
在这里可以加功能啊...
------> 1 2 3
p1.say_hi(4, 5, 6)
在这里可以加功能啊...
------> 4 5 6
```



# __setitem__和__getitem和__delitem__

```
class Foo:
    def __init__(self, name):
        self.name = name

    def __getitem__(self, item):
        print('getitem执行', self.__dict__[item])

    def __setitem__(self, key, value):
        print('setitem执行')
        self.__dict__[key] = value

    def __delitem__(self, key):
        print('del obj[key]时，delitem执行')
        self.__dict__.pop(key)

    def __delattr__(self, item):
        print('del obj.key时，delattr执行')
        self.__dict__.pop(item)


f1 = Foo('sb')
```

## **setitem**

- 中括号赋值时触发

```
f1['age'] = 18
f1['age1'] = 19
setitem执行
setitem执行
```

## **getitem**

- 中括号取值时触发

```
f1['age']
getitem执行 18
f1['name'] = 'tank'
setitem执行
```

## **delitem**与**delattr**

- **delitem**：中括号删除时触发
- **delattr**：.删除时触发

```
del f1.age1
del f1['age']
del obj.key时，delattr执行
del obj[key]时，delitem执行
print(f1.__dict__)
{'name': 'tank'}
```



# __format__

## **format**

- 自定制格式化字符串

```
date_dic = {
    'ymd': '{0.year}:{0.month}:{0.day}',
    'dmy': '{0.day}/{0.month}/{0.year}',
    'mdy': '{0.month}-{0.day}-{0.year}',
}


class Date:
    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day

    def __format__(self, format_spec):
        # 默认打印ymd的{0.year}:{0.month}:{0.day}格式
        if not format_spec or format_spec not in date_dic:
            format_spec = 'ymd'
        fmt = date_dic[format_spec]
        return fmt.format(self)


d1 = Date(2016, 12, 29)
print(format(d1))
2016:12:29
print('{:mdy}'.format(d1))
12-29-2016
```

# __del__
## **del**

- **del**也称之为析构方法
- **del**会在对象被删除之前自动触发



```
class People:
    def __init__(self, name, age):
        self.name = name
        self.age = age
        self.f = open('test.txt', 'w', encoding='utf-8')

    def __del__(self):
        print('run======>')
        # 做回收系统资源相关的事情
        self.f.close()


obj = People('egon', 18)
del obj  # del obj会间接删除f的内存占用，但是还需要自定制__del__删除文件的系统占用
print('主')
run=-====>
主
```



# __slots__
## 什么是**slots**

- **slots**是一个类变量，变量值可以是列表，元祖，或者可迭代对象，也可以是一个字符串(意味着所有实例只有一个数据属性)
- 使用点来访问属性本质就是在访问类或者对象的**dict**属性字典(类的字典是共享的，而每个实例的是独立的)



## 为什么用**slots**

- 字典会占用大量内存，如果你有一个属性很少的类，但是有很多实例，为了节省内存可以使用**slots**取代实例的**dict**

- 当你定义**slots**后，**slots**就会为实例使用一种更加紧凑的内部表示。实例通过一个很小的固定大小的数组来构建，而不是为每个实例定义一个字典，这跟元组或列表很类似。在**slots**中列出的属性名在内部被映射到这个数组的指定小标上。使用**slots**一个不好的地方就是我们不能再给实例添加新的属性了，只能使用在**slots**中定义的那些属性名。

  ```
  class Foo:
      __slots__='x'
  
  
  f1=Foo()
  f1.x=1
  f1.y=2#报错
  print(f1.__slots__) #f1不再有__dict__
  
  class Bar:
      __slots__=['x','y']
      
  n=Bar()
  n.x,n.y=1,2
  n.z=3#报错
  ```

- 注意：**slots**的很多特性都依赖于普通的基于字典的实现。另外，定义了**slots**后的类不再 支持一些普通类特性了，比如多继承。大多数情况下，你应该只在那些经常被使用到 的用作数据结构的类上定义**slots**比如在程序中需要创建某个类的几百万个实例对象 。

- 关于**slots**的一个常见误区是它可以作为一个封装工具来防止用户给实例增加新的属性。尽管使用**slots**可以达到这样的目的，但是这个并不是它的初衷。它更多的是用来作为一个内存优化工具。

## 刨根问底

```
class Foo:
    __slots__=['name','age']

f1=Foo()
f1.name='alex'
f1.age=18
print(f1.__slots__)
['name', 'age']
f2=Foo()
f2.name='egon'
f2.age=19
print(f2.__slots__)
['name', 'age']
```

- f1与f2都没有属性字典**dict**了，统一归**slots**管，节省内存

```
print(Foo.__dict__)
{'__module__': '__main__', '__slots__': ['name', 'age'], 'age': <member 'age' of 'Foo' objects>, 'name': <member 'name' of 'Foo' objects>, '__doc__': None}
```



# __doc__
## **doc**

- 返回类的注释信息

```
class Foo:
    '我是描述信息'
    pass


print(Foo.__doc__)
我是描述信息
```

- 该属性无法被继承

```
class Foo:
    '我是描述信息'
    pass

class Bar(Foo):
    pass
print(Bar.__doc__) #该属性无法继承给子类
None
```



# __call__
## **call**

- 对象后面加括号时，触发执行。
- 注：构造方法的执行是由创建对象触发的，即：对象 = 类名() ；而对于 **call** 方法的执行是由对象后加括号触发的，即：对象() 或者 类()()

```
class Foo:
    def __init__(self):
        print('__init__触发了')

    def __call__(self, *args, **kwargs):

        print('__call__触发了')


obj = Foo()  # 执行 __init__
__init__触发了
obj()  # 执行 __call__
__call__
```

# __init__、__new__
我们首先得从**new**(cls[,…])的参数说说起，**new**方法的第一个参数是这个类，而其余的参数会在调用成功后全部传递给**init**方法初始化，这一下子就看出了谁是老子谁是小子的关系。

所以，**new**方法（第一个执行）先于**init**方法执行：

```
class A:
    pass


class B(A):
    def __new__(cls):
        print("__new__方法被执行")
        return super().__new__(cls)

    def __init__(self):
        print("__init__方法被执行")


b = B()
__new__方法被执行
__init__方法被执行
```

我们比较两个方法的参数，可以发现**new**方法是传入类(cls)，而**init**方法传入类的实例化对象(self)，而有意思的是，**new**方法返回的值就是一个实例化对象（ps:如果**new**方法返回None，则**init**方法不会被执行，并且返回值只能调用父类中的**new**方法，而不能调用毫无关系的类的**new**方法）。我们可以这么理解它们之间的关系，**new**是开辟疆域的大将军，而**init**是在这片疆域上辛勤劳作的小老百姓，只有**new**执行完后，开辟好疆域后，**init**才能工作。

绝大多数情况下，我们都不需要自己重写**new**方法，但在当继承一个不可变的类型（例如str类,int类等）时，它的特性就尤显重要了。我们举下面这个例子：

```
class CapStr(str):
    def __init__(self, string):
        string = string.upper()


a = CapStr("I love China!")
print(a)
I love China!
class CapStr(str):
    def __new__(cls, string):
        string = string.upper()
        return super().__new__(cls, string)


a = CapStr("I love China!")
print(a)
I LOVE CHINA!
```

我们可以根据上面的理论可以这样分析，我们知道字符串是不可改变的，所以第一个例子中，传入的字符串相当于已经被打下的疆域，而这块疆域除了将军其他谁也无法改变，**init**只能在这块领地上干瞪眼，此时这块疆域就是”I love China!“。而第二个例子中，**new**大将军重新去开辟了一块疆域，所以疆域上的内容也发生了变化，此时这块疆域变成了”I LOVE CHINA!“。



**小结：*****\*new\**和\**init\**想配合才是python中真正的类构造器。**

# __str__ 、__repr__

## **str**

- 打印时触发

```
class Foo:
    pass


obj = Foo()
print(obj)
<__main__.Foo object at 0x10d2b8f98>
dic = {'a': 1}  # d = dict({'x':1})
print(dic)
{'a': 1}
```

- obj和dic都是实例化的对象，但是obj打印的是内存地址，而dic打印的是有用的信息，很明显dic的打印是非常好的

```
class Foo:
    def __init__(self, name, age):
        """对象实例化的时候自动触发"""
        self.name = name
        self.age = age

    def __str__(self):
        print('打印的时候自动触发，但是其实不需要print即可打印')
        return f'{self.name}:{self.age}'  # 如果不返回字符串类型，则会报错


obj = Foo('nick', 18)
print(obj)  # obj.__str__() # 打印的时候就是在打印返回值
打印的时候自动触发，但是其实不需要print即可打印
nick:18
obj2 = Foo('tank', 30)
print(obj2)
打印的时候自动触发，但是其实不需要print即可打印
tank:30
```

## **repr**

- str函数或者print函数—>obj.**str**()
- repr或者交互式解释器—>obj.**repr**()
- 如果**str**没有被定义，那么就会使用**repr**来代替输出
- 注意：这俩方法的返回值必须是字符串，否则抛出异常

```
class School:
    def __init__(self, name, addr, type):
        self.name = name
        self.addr = addr
        self.type = type

    def __repr__(self):
        return 'School(%s,%s)' % (self.name, self.addr)

    def __str__(self):
        return '(%s,%s)' % (self.name, self.addr)


s1 = School('oldboy1', '北京', '私立')
print('from repr: ', repr(s1))
from repr:  School(oldboy1,北京)
print('from str: ', str(s1))
from str:  (oldboy1,北京)
print(s1)
(oldboy1,北京)
s1  # jupyter属于交互式
School(oldboy1,北京)
```

# __next__、__iter__
## 简单示例

- 死循环

```
class Foo:
    def __init__(self, x):
        self.x = x

    def __iter__(self):
        return self

    def __next__(self):
        self.x += 1
        return self.x


f = Foo(3)
for i in f:
    print(i)
```

## StopIteration异常版

- 加上StopIteration异常

```
class Foo:
    def __init__(self, start, stop):
        self.num = start
        self.stop = stop

    def __iter__(self):
        return self

    def __next__(self):
        if self.num >= self.stop:
            raise StopIteration
        n = self.num
        self.num += 1
        return n


f = Foo(1, 5)
from collections import Iterable, Iterator
print(isinstance(f, Iterator))
True
for i in Foo(1, 5):
    print(i)
1
2
3
4
```

## 模拟range

```
class Range:
    def __init__(self, n, stop, step):
        self.n = n
        self.stop = stop
        self.step = step

    def __next__(self):
        if self.n >= self.stop:
            raise StopIteration
        x = self.n
        self.n += self.step
        return x

    def __iter__(self):
        return self


for i in Range(1, 7, 3):
    print(i)
1
4
```

## 斐波那契数列

```
class Fib:
    def __init__(self):
        self._a = 0
        self._b = 1

    def __iter__(self):
        return self

    def __next__(self):
        self._a, self._b = self._b, self._a + self._b
        return self._a


f1 = Fib()
for i in f1:
    if i > 100:
        break
    print('%s ' % i, end='')
1 1 2 3 5 8 13 21 34 55 89
```



# __module__和__class__

```
# lib/aa.py
class C:
    def __init__(self):
        self.name = 'SB'
# index.py

from lib.aa import C

obj = C()
```

## **module**

- **module** 表示当前操作的对象在那个模块

```
print(obj.__module__)  # 输出 lib.aa，即：输出模块
```

## **class**

- **class**表示当前操作的对象的类是什么

```
print(obj.__class__)  # 输出 lib.aa.C，即：输出类
```

# 文件上下文管理(__enter__和__exit__)
- 我们知道在操作文件对象的时候可以这么写

```
with open('a.txt') as f:
    '代码块'
```

- 上述叫做上下文管理协议，即with语句，为了让一个对象兼容with语句，必须在这个对象的类中声明**enter**和**exit**方法

## 上下文管理协议

```
class Open:
    def __init__(self, name):
        self.name = name

    def __enter__(self):
        print('出现with语句，对象的__enter__被触发，有返回值则赋值给as声明的变量')
        # return self
    def __exit__(self, exc_type, exc_val, exc_tb):
        print('with中代码块执行完毕时执行我啊')


with Open('a.txt') as f:
    print('=====>执行代码块')
    # print(f,f.name)
出现with语句,对象的__enter__被触发,有返回值则赋值给as声明的变量
=====>执行代码块
with中代码块执行完毕时执行我啊
```

- **exit**()中的三个参数分别代表异常类型，异常值和追溯信息,with语句中代码块出现异常，则with后的代码都无法执行

```
class Open:
    def __init__(self, name):
        self.name = name

    def __enter__(self):
        print('出现with语句，对象的__enter__被触发，有返回值则赋值给as声明的变量')

    def __exit__(self, exc_type, exc_val, exc_tb):
        print('with中代码块执行完毕时执行我啊')
        print(exc_type)
        print(exc_val)
        print(exc_tb)


try:
    with Open('a.txt') as f:
        print('=====>执行代码块')
        raise AttributeError('***着火啦，救火啊***')
except Exception as e:
    print(e)
出现with语句，对象的__enter__被触发，有返回值则赋值给as声明的变量
=====>执行代码块
with中代码块执行完毕时执行我啊
<class 'AttributeError'>
***着火啦，救火啊***
<traceback object at 0x1065f1f88>
***着火啦，救火啊***
```

- 如果__exit()返回值为True,那么异常会被清空，就好像啥都没发生一样，with后的语句正常执行

```
class Open:
    def __init__(self, name):
        self.name = name

    def __enter__(self):
        print('出现with语句，对象的__enter__被触发，有返回值则赋值给as声明的变量')

    def __exit__(self, exc_type, exc_val, exc_tb):
        print('with中代码块执行完毕时执行我啊')
        print(exc_type)
        print(exc_val)
        print(exc_tb)
        return True


with Open('a.txt') as f:
    print('=====>执行代码块')
    raise AttributeError('***着火啦，救火啊***')
print('0' * 100)  #------------------------------->会执行
出现with语句，对象的__enter__被触发，有返回值则赋值给as声明的变量
=====>执行代码块
with中代码块执行完毕时执行我啊
<class 'AttributeError'>
***着火啦，救火啊***
<traceback object at 0x1062ab048>
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
```

## 模拟open

```
class Open:
    def __init__(self, filepath, mode='r', encoding='utf-8'):
        self.filepath = filepath
        self.mode = mode
        self.encoding = encoding

    def __enter__(self):
        # print('enter')
        self.f = open(self.filepath, mode=self.mode, encoding=self.encoding)
        return self.f

    def __exit__(self, exc_type, exc_val, exc_tb):
        # print('exit')
        self.f.close()
        return True

    def __getattr__(self, item):
        return getattr(self.f, item)


with Open('a.txt', 'w') as f:
    print(f)
    f.write('aaaaaa')
    f.wasdf  #抛出异常，交给__exit__处理
<_io.TextIOWrapper name='a.txt' mode='w' encoding='utf-8'>
```

## 优点

1. 使用with语句的目的就是把代码块放入with中执行，with结束后，自动完成清理工作，无须手动干预
2. 在需要管理一些资源比如文件，网络连接和锁的编程环境中，可以在**exit**中定制自动释放资源的机制，你无须再去关系这个问题，这将大有用处




# 元类
## 引言

- 元类属于python面向对象编程的深层魔法，99%的人都不得要领，一些自以为搞明白元类的人其实也只是自圆其说、点到为止，从对元类的控制上来看就破绽百出、逻辑混乱，今天我就来带大家来深度了解python元类的来龙去脉。
- 笔者深入浅出的背后是对技术一日复一日的执念，希望可以大家可以尊重原创，为大家能因此文而解开对元类所有的疑惑而感到开心！！！

## 什么是元类

- 在python中一切皆对象，那么我们用class关键字定义的类本身也是一个对象，负责产生该对象的类称之为元类，即元类可以简称为类的类

```
class Foo:  # Foo=元类()
    pass
```

![114-元类metaclass-类的创建.png](https://tva1.sinaimg.cn/large/0081Kckwgy1gm2qxadyxlj30i70463ye.jpg)

## 为什么用元类

- 元类是负责产生类的，所以我们学习元类或者自定义元类的目的：是为了控制类的产生过程，还可以控制对象的产生过程

## 内置函数exec(储备)

```
cmd = """
x=1
print('exec函数运行了')
def func(self):
    pass
"""
class_dic = {}
# 执行cmd中的代码，然后把产生的名字丢入class_dic字典中
exec(cmd, {}, class_dic)
exec函数运行了
print(class_dic)
{'x': 1, 'func': <function func at 0x10a0bc048>}
```

## class创建类

- 如果说类也是对象，那么用class关键字的去创建类的过程也是一个实例化的过程，该实例化的目的是为了得到一个类，调用的是元类
- 用class关键字创建一个类，用的默认的元类type，因此以前说不要用type作为类别判断

```
class People:  # People=type(...)
    country = 'China'

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def eat(self):
        print('%s is eating' % self.name)
print(type(People))
<class 'type'>
```



###  type实现

- 创建类的3个要素：类名，基类，类的名称空间
- People = type(类名，基类，类的名称空间)

```
class_name = 'People'  # 类名

class_bases = (object, )  # 基类

# 类的名称空间
class_dic = {}
class_body = """
country='China'
def __init__(self,name,age):
    self.name=name
    self.age=age
def eat(self):
    print('%s is eating' %self.name)
"""

exec(
    class_body,
    {},
    class_dic,
)
print(class_name)
People
print(class_bases)
(<class 'object'>,)
print(class_dic)  # 类的名称空间
{'country': 'China', '__init__': <function __init__ at 0x10a0bc048>, 'eat': <function eat at 0x10a0bcd08>}
```

- People = type(类名，基类，类的名称空间)

```
People1 = type(class_name, class_bases, class_dic)
print(People1)
<class '__main__.People'>
obj1 = People1(1, 2)
obj1.eat()
1 is eating
```

- class创建的类的调用

```
print(People)
<class '__main__.People'>
obj = People1(1, 2)
obj.eat()
1 is eating
```

## 自定义元类控制类的创建

- 使用自定义的元类

```
class Mymeta(type):  # 只有继承了type类才能称之为一个元类，否则就是一个普通的自定义类
    def __init__(self, class_name, class_bases, class_dic):
        print('self:', self)  # 现在是People
        print('class_name:', class_name)
        print('class_bases:', class_bases)
        print('class_dic:', class_dic)
        super(Mymeta, self).__init__(class_name, class_bases,
                                     class_dic)  # 重用父类type的功能
```

- 分析用class自定义类的运行原理（而非元类的的运行原理）：
  1. 拿到一个字符串格式的类名class_name=’People’
  2. 拿到一个类的基类们class_bases=(obejct,)
  3. 执行类体代码，拿到一个类的名称空间class_dic={…}
  4. 调用People=type(class_name,class_bases,class_dic)

```
class People(object, metaclass=Mymeta):  # People=Mymeta(类名,基类们,类的名称空间)
    country = 'China'

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def eat(self):
        print('%s is eating' % self.name)
self: <class '__main__.People'>
class_name: People
class_bases: (<class 'object'>,)
class_dic: {'__module__': '__main__', '__qualname__': 'People', 'country': 'China', '__init__': <function People.__init__ at 0x10a0bcbf8>, 'eat': <function People.eat at 0x10a0bc2f0>}
```

###  应用

- 自定义元类控制类的产生过程，类的产生过程其实就是元类的调用过程
- 我们可以控制类必须有文档，可以使用如下的方式实现

```
class Mymeta(type):  # 只有继承了type类才能称之为一个元类，否则就是一个普通的自定义类
    def __init__(self, class_name, class_bases, class_dic):
        if class_dic.get('__doc__') is None or len(
                class_dic.get('__doc__').strip()) == 0:
            raise TypeError('类中必须有文档注释，并且文档注释不能为空')
        if not class_name.istitle():
            raise TypeError('类名首字母必须大写')
        super(Mymeta, self).__init__(class_name, class_bases,
                                     class_dic)  # 重用父类的功能
try:

    class People(object, metaclass=Mymeta
                 ):  #People  = Mymeta('People',(object,),{....})
        #     """这是People类"""
        country = 'China'

        def __init__(self, name, age):
            self.name = name
            self.age = age

        def eat(self):
            print('%s is eating' % self.name)
except Exception as e:
    print(e)
类中必须有文档注释，并且文档注释不能为空
```

## **call**(储备)

- 要想让obj这个对象变成一个可调用的对象，需要在该对象的类中定义一个方法、、**call**方法，该方法会在调用对象时自动触发

```
class Foo:
    def __call__(self, *args, **kwargs):
        print(args)
        print(kwargs)
        print('__call__实现了，实例化对象可以加括号调用了')


obj = Foo()
obj('lqz', age=18)
('lqz',)
{'age': 18}
__call__实现了，实例化对象可以加括号调用了
```

## **new**(储备)

我们之前说类实例化第一个调用的是**init**，但**init**其实不是实例化一个类的时候第一个被调用 的方法。当使用 Persion(name, age) 这样的表达式来实例化一个类时，最先被调用的方法 其实是 **new** 方法。

**new**方法接受的参数虽然也是和**init**一样，但**init**是在类实例创建之后调用，而 **new**方法正是创建这个类实例的方法。

注意：***\*new\**() 函数只能用于从object继承的新式类。**

```
class A:
    pass


class B(A):
    def __new__(cls):
        print("__new__方法被执行")
        return cls.__new__(cls)

    def __init__(self):
        print("__init__方法被执行")


b = B()
```

## 自定义元类控制类的实例化

```
class Mymeta(type):
    def __call__(self, *args, **kwargs):
        print(self)  # self是People
        print(args)  # args = ('lqz',)
        print(kwargs)  # kwargs = {'age':18}
        # return 123
        # 1. 先造出一个People的空对象，申请内存空间
        # __new__方法接受的参数虽然也是和__init__一样，但__init__是在类实例创建之后调用，而 __new__方法正是创建这个类实例的方法。
        obj = self.__new__(self)  # 虽然和下面同样是People，但是People没有，找到的__new__是父类的
        # 2. 为该对空对象初始化独有的属性
        self.__init__(obj, *args, **kwargs)
        # 3. 返回一个初始化好的对象
        return obj
```

- People = Mymeta()，People()则会触发**call**

```
class People(object, metaclass=Mymeta):
    country = 'China'

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def eat(self):
        print('%s is eating' % self.name)


#     在调用Mymeta的__call__的时候，首先会找自己（如下函数）的，自己的没有才会找父类的
#     def __new__(cls, *args, **kwargs):
#         # print(cls)  # cls是People
#         # cls.__new__(cls) # 错误，无限死循环，自己找自己的，会无限递归
#         obj = super(People, cls).__new__(cls)  # 使用父类的，则是去父类中找__new__
#         return obj
```

- 类的调用，即类实例化就是元类的调用过程，可以通过元类Mymeta的**call**方法控制
- 分析：调用Pepole的目的
  1. 先造出一个People的空对象
  2. 为该对空对象初始化独有的属性
  3. 返回一个初始化好的对象

```
obj = People('lqz', age=18)
<class '__main__.People'>
('lqz',)
{'age': 18}
print(obj.__dict__)
{'name': 'lqz', 'age': 18}
```

## 自定义元类后类的继承顺序

结合python继承的实现原理+元类重新看属性的查找应该是什么样子呢？？？

在学习完元类后，其实我们用class自定义的类也全都是对象（包括object类本身也是元类type的 一个实例，可以用type(object)查看），我们学习过继承的实现原理，如果把类当成对象去看，将下述继承应该说成是：对象OldboyTeacher继承对象Foo，对象Foo继承对象Bar，对象Bar继承对象object

```
class Mymeta(type):  # 只有继承了type类才能称之为一个元类，否则就是一个普通的自定义类
    n = 444

    def __call__(self, *args,
                 **kwargs):  #self=<class '__main__.OldboyTeacher'>
        obj = self.__new__(self)
        self.__init__(obj, *args, **kwargs)
        return obj


class Bar(object):
    n = 333


class Foo(Bar):
    n = 222


class OldboyTeacher(Foo, metaclass=Mymeta):
    n = 111

    school = 'oldboy'

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def say(self):
        print('%s says welcome to the oldboy to learn Python' % self.name)


print(
    OldboyTeacher.n
)  # 自下而上依次注释各个类中的n=xxx，然后重新运行程序，发现n的查找顺序为OldboyTeacher->Foo->Bar->object->Mymeta->type
111
print(OldboyTeacher.n)
111
```

- 查找顺序：
  1. 先对象层：OldoyTeacher->Foo->Bar->object
  2. 然后元类层：Mymeta->type

依据上述总结，我们来分析下元类Mymeta中**call**里的self.**new**的查找

```
class Mymeta(type):
    n = 444

    def __call__(self, *args,
                 **kwargs):  #self=<class '__main__.OldboyTeacher'>
        obj = self.__new__(self)
        print(self.__new__ is object.__new__)  #True


class Bar(object):
    n = 333

    # def __new__(cls, *args, **kwargs):
    #     print('Bar.__new__')


class Foo(Bar):
    n = 222

    # def __new__(cls, *args, **kwargs):
    #     print('Foo.__new__')


class OldboyTeacher(Foo, metaclass=Mymeta):
    n = 111

    school = 'oldboy'

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def say(self):
        print('%s says welcome to the oldboy to learn Python' % self.name)

    # def __new__(cls, *args, **kwargs):
    #     print('OldboyTeacher.__new__')


OldboyTeacher('lqz',
              18)  # 触发OldboyTeacher的类中的__call__方法的执行，进而执行self.__new__开始查找
```

总结，Mymeta下的**call**里的self.**new**在OldboyTeacher、Foo、Bar里都没有找到**new**的情况下，会去找object里的**new**，而object下默认就有一个**new**，所以即便是之前的类均未实现**new**,也一定会在object中找到一个，根本不会、也根本没必要再去找元类Mymeta->type中查找**new**















