---
title: super
description: 
published: true
date: 2023-03-07T01:34:33.051Z
tags: 
editor: markdown
dateCreated: 2023-02-26T04:49:36.562Z
---

# 单独调用父类的方法

需求：编写一个类，然后再写一个子类进行继承，使用子类去调用父类的方法1。

使用方法1打印： 胖子老板，来包槟榔。

> 那么先写一个胖子老板的父类，执行一下：

```
class FatFather(object):
    def __init__(self, name):
        print('FatFather的init开始被调用')
        self.name = name
        print('FatFather的name是%s' % self.name)
        print('FatFather的init调用结束')


def main():
    ff = FatFather("胖子老板的父亲")
```

运行一下这个胖子老板父类的构造方法**init** 如下：

```
if __name__ == "__main__":
    main()
FatFather的init开始被调用
FatFather的name是胖子老板的父亲
FatFather的init调用结束
```

> 好了，那么下面来写一个子类，也就是胖子老板类，继承上面的类

```
# 胖子老板的父类
class FatFather(object):
    def __init__(self, name):
        print('FatFather的init开始被调用')
        self.name = name
        print('调用FatFather类的name是%s' % self.name)
        print('FatFather的init调用结束')


# 胖子老板类 继承 FatFather 类
class FatBoss(FatFather):
    def __init__(self, name, hobby):
        print('胖子老板的类被调用啦！')
        self.hobby = hobby
        FatFather.__init__(self, name)  # 直接调用父类的构造方法
        print("%s 的爱好是 %s" % (name, self.hobby))


def main():
    #ff = FatFather("胖子老板的父亲")
    fatboss = FatBoss("胖子老板", "打斗地主")
```

在这上面的代码中，我使用FatFather.**init**(self,name)直接调用父类的方法。
运行结果如下：

```
if __name__ == "__main__":
    main()
胖子老板的类被调用啦！
FatFather的init开始被调用
调用FatFather类的name是胖子老板
FatFather的init调用结束
胖子老板 的爱好是 打斗地主
```

# super() 方法基本概念

> 除了直接使用 FatFather.**init**(self,name) 的方法，还可以使用super()方法来调用。

那么首先需要看super()方法的描述和语法理解一下super() 方法的使用。

## 描述

**super() 函数是用于调用父类(超类)的一个方法。**

super 是用来解决多重继承问题的，直接用类名调用父类方法在使用单继承的时候没问题，但是如果使用多继承，会涉及到查找顺序（MRO）、重复调用（钻石继承）等种种问题。

MRO 就是类的方法解析顺序表, 其实也就是继承父类方法时的顺序表。

##  语法

以下是 super() 方法的语法:

```
super(type[, object-or-type])
```

> 参数

- type – 类
- object-or-type – 类，一般是 self

> Python3.x 和 Python2.x 的一个区别是: Python 3 可以使用直接使用 super().xxx 代替 super(Class, self).xxx :

- Python3.x 实例：

```
class A:
    pass
class B(A):
    def add(self, x):
        super().add(x)
```

- Python2.x 实例：

```
class A(object):   # Python2.x 记得继承 object
    pass
class B(A):
    def add(self, x):
        super(B, self).add(x)
```

# 单继承使用super()

- 使用super() 方法来改写刚才胖子老板继承父类的 **init** 构造方法

```
# 胖子老板的父类
class FatFather(object):
    def __init__(self, name):
        print('FatFather的init开始被调用')
        self.name = name
        print('调用FatFather类的name是%s' % self.name)
        print('FatFather的init调用结束')


# 胖子老板类 继承 FatFather 类
class FatBoss(FatFather):
    def __init__(self, name, hobby):
        print('胖子老板的类被调用啦！')
        self.hobby = hobby
        #FatFather.__init__(self,name)   # 直接调用父类的构造方法
        super().__init__(name)
        print("%s 的爱好是 %s" % (name, self.hobby))


def main():
    #ff = FatFather("胖子老板的父亲")
    fatboss = FatBoss("胖子老板", "打斗地主")
```

从上面使用super方法的时候，因为是单继承，直接就可以使用了。
运行如下：

```
if __name__ == "__main__":
    main()
胖子老板的类被调用啦！
FatFather的init开始被调用
调用FatFather类的name是胖子老板
FatFather的init调用结束
胖子老板 的爱好是 打斗地主
```

那么为什么说**单继承**直接使用就可以呢？因为super()方法如果多继承的话，会涉及到一个MRO(**继承父类方法时的顺序表**) 的调用排序问题。

> 下面可以打印一下看看单继承的MRO顺序(FatBoss.**mro**)。

```
# 胖子老板的父类
class FatFather(object):
    def __init__(self, name):
        print('FatFather的init开始被调用')
        self.name = name
        print('调用FatFather类的name是%s' % self.name)
        print('FatFather的init调用结束')


# 胖子老板类 继承 FatFather 类
class FatBoss(FatFather):
    def __init__(self, name, hobby):
        print('胖子老板的类被调用啦！')
        self.hobby = hobby
        #FatFather.__init__(self,name)   # 直接调用父类的构造方法
        super().__init__(name)
        print("%s 的爱好是 %s" % (name, self.hobby))


def main():

    print("打印FatBoss类的MRO")
    print(FatBoss.__mro__)

    print()
    print("=========== 下面按照 MRO 顺序执行super方法 =============")
    fatboss = FatBoss("胖子老板", "打斗地主")
```

上面的代码使用 FatBoss.**mro** 可以打印出 FatBoss这个类经过 python解析器的 C3算法计算过后的继承调用顺序。
运行如下：

```
if __name__ == "__main__":
    main()
打印FatBoss类的MRO
(<class '__main__.FatBoss'>, <class '__main__.FatFather'>, <class 'object'>)

=========== 下面按照 MRO 顺序执行super方法 =============
胖子老板的类被调用啦！
FatFather的init开始被调用
调用FatFather类的name是胖子老板
FatFather的init调用结束
胖子老板 的爱好是 打斗地主
```

从上面的结果 (<class ‘**main**.FatBoss’>, <class ‘**main**.FatFather’>, <class ‘object’>) 可以看出，super() 方法在 FatBoss 会直接调用父类是 FatFather ，所以单继承是没问题的。

那么如果多继承的话，会有什么问题呢？

## 多继承使用super()

假设再写一个胖子老板的女儿类，和 胖子老板的老婆类，此时女儿需要同时继承 两个类（胖子老板类，胖子老板老婆类）。

因为胖子老板有一个爱好，胖子老板的老婆需要干活干家务，那么女儿需要帮忙同时兼顾。

此时女儿就是需要继承使用这两个父类的方法了，那么该如何去写呢？

下面来看看实现代码：

```
# 胖子老板的父类
class FatFather(object):
    def __init__(self, name, *args, **kwargs):
        print()
        print("=============== 开始调用 FatFather  ========================")
        print('FatFather的init开始被调用')
        self.name = name
        print('调用FatFather类的name是%s' % self.name)
        print('FatFather的init调用结束')
        print()
        print("=============== 结束调用 FatFather  ========================")


# 胖子老板类 继承 FatFather 类
class FatBoss(FatFather):
    def __init__(self, name, hobby, *args, **kwargs):
        print()
        print("=============== 开始调用 FatBoss  ========================")
        print('胖子老板的类被调用啦！')
        #super().__init__(name)
        ## 因为多继承传递的参数不一致，所以使用不定参数
        super().__init__(name, *args, **kwargs)
        print("%s 的爱好是 %s" % (name, hobby))
        print()
        print("=============== 结束调用 FatBoss  ========================")


# 胖子老板的老婆类 继承 FatFather类
class FatBossWife(FatFather):
    def __init__(self, name, housework, *args, **kwargs):
        print()
        print("=============== 开始调用 FatBossWife  ========================")
        print('胖子老板的老婆类被调用啦！要学会干家务')
        #super().__init__(name)
        ## 因为多继承传递的参数不一致，所以使用不定参数
        super().__init__(name, *args, **kwargs)
        print("%s 需要干的家务是 %s" % (name, housework))
        print()
        print("=============== 结束调用 FatBossWife  ========================")


# 胖子老板的女儿类 继承 FatBoss FatBossWife类
class FatBossGril(FatBoss, FatBossWife):
    def __init__(self, name, hobby, housework):
        print('胖子老板的女儿类被调用啦！要学会干家务，还要会帮胖子老板斗地主')
        super().__init__(name, hobby, housework)


def main():

    print("打印FatBossGril类的MRO")
    print(FatBossGril.__mro__)

    print()
    print("=========== 下面按照 MRO 顺序执行super方法 =============")
    gril = FatBossGril("胖子老板", "打斗地主", "拖地")
```

运行结果如下：

```
if __name__ == "__main__":
    main()
打印FatBossGril类的MRO
(<class '__main__.FatBossGril'>, <class '__main__.FatBoss'>, <class '__main__.FatBossWife'>, <class '__main__.FatFather'>, <class 'object'>)

=========== 下面按照 MRO 顺序执行super方法 =============
胖子老板的女儿类被调用啦！要学会干家务，还要会帮胖子老板斗地主

=============== 开始调用 FatBoss  ========================
胖子老板的类被调用啦！

=============== 开始调用 FatBossWife  ========================
胖子老板的老婆类被调用啦！要学会干家务

=============== 开始调用 FatFather  ========================
FatFather的init开始被调用
调用FatFather类的name是胖子老板
FatFather的init调用结束

=============== 结束调用 FatFather  ========================
胖子老板 需要干的家务是 拖地

=============== 结束调用 FatBossWife  ========================
胖子老板 的爱好是 打斗地主

=============== 结束调用 FatBoss  ========================
```

从上面的运行结果来看，我特意给每个类的调用开始以及结束都进行打印标识，可以看到。

每个类开始调用是根据MRO顺序进行开始，然后逐个进行结束的。

还有就是由于因为需要继承不同的父类，参数不一定。

所以，所有的父类都应该加上不定参数*args , **kwargs ，不然参数不对应是会报错的。

# 注意事项

- super().**init**相对于类名.**init**，在单继承上用法基本无差
- 但在多继承上有区别，super方法能保证每个父类的方法只会执行一次，而使用类名的方法会导致方法被执行多次，可以尝试写个代码来看输出结果
- 多继承时，使用super方法，对父类的传参数，应该是由于python中super的算法导致的原因，必须把参数全部传递，否则会报错
- 单继承时，使用super方法，则不能全部传递，只能传父类方法所需的参数，否则会报错
- 多继承时，相对于使用类名.**init**方法，要把每个父类全部写一遍, 而使用super方法，只需写一句话便执行了全部父类的方法，这也是为何多继承需要全部传参的一个原因

