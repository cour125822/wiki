---
title: Class类
description: 
published: true
date: 2023-03-26T08:06:04.069Z
tags: 
editor: markdown
dateCreated: 2023-03-26T07:47:27.798Z
---

ES6提供了更接近传统语言的写法，引入了Class（类）这个概念，作为对象的模板。通过class关键字，可以定义类。基本上，ES6的class可以看作只是一个语法糖，它的绝大部分功能，ES5都可以做到，新的class写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。上面的代码用ES6的“类”改写，就是下面这样。

```ts
//定义类
class Person {
    constructor () {
 
    }
    run () {
        
    }
}
```

# 在TS 中定义类
![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303261233767.png)



在TypeScript是不允许直接在constructor 定义变量的 需要在constructor上面先声明
![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303261233567.png)



这样引发了第二个问题你如果了定义了变量不用 也会报错 通常是给个默认值 或者 进行赋值

![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303261233795.png)
![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303261234215.png)


 恭喜你已经学会了在class中 如何定义变量

# 2.类的修饰符
总共有三个 public private protected
![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303261234190.png)


使用public 修饰符 可以让你定义的变量 内部访问 也可以外部访问 如果不写默认就是public
![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303261234288.png)

使用  private 修饰符 代表定义的变量私有的只能在内部访问 不能在外部访问
![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303261234139.png)
![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303261234683.png)



  使用  protected 修饰符 代表定义的变量私有的只能在内部和继承的子类中访问 不能在外部访问

```ts
class Person {
    public name:string
    private age:number 
    protected some:any
    constructor (name:string,ages:number,some:any) {
       this.name = name
       this.age = ages
       this.some = some
    }
    run () {
 
    }
}
 
class Man extends Person{
    constructor () {
        super("张三",18,1)
        console.log(this.some)
    }
    create () {
       console.log(this.some)
    }
}
let xiaoman = new Person('小满',18,1)
let man = new Man()
man.some

```


# static 静态属性 和 静态方法
![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303261235274.png)

我们用static 定义的属性 不可以通过this 去访问 只能通过类名去调用

![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303261235786.png)


static 静态函数 同样也是不能通过this 去调用 也是通过类名去调用

![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303261236142.png)


需注意： 如果两个函数都是static 静态的是可以通过this互相调用
![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303261236702.png)



# interface 定义 类
![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303261237991.png)
ts interface 定义类 使用关键字 implements   后面跟interface的名字多个用逗号隔开 继承还是用extends

```ts
interface PersonClass {
    get(type: boolean): boolean
}
 
interface PersonClass2{
    set():void,
    asd:string
}
 
class A {
    name: string
    constructor() {
        this.name = "123"
    }
}
 
class Person extends A implements PersonClass,PersonClass2 {
    asd: string
    constructor() {
        super()
        this.asd = '123'
    }
    get(type:boolean) {
        return type
    }
    set () {
 
    }
}
```

# 抽象类 
应用场景如果你写的类实例化之后毫无用处此时我可以把他定义为抽象类

或者你也可以把他作为一个基类-> 通过继承一个派生类去实现基类的一些方法

我们看例子

下面这段代码会报错抽象类无法被实例化
```ts
abstract class A {
   public name:string
   
}
 
new A()
```
例子2

我们在A类定义了 getName 抽象方法但为实现

我们B类实现了A定义的抽象方法 如不实现就不报错 我们定义的抽象方法必须在派生类实现
```ts
abstract class A {
   name: string
   constructor(name: string) {
      this.name = name;
   }
   print(): string {
      return this.name
   }
 
   abstract getName(): string
}
 
class B extends A {
   constructor() {
      super('小满')
   }
   getName(): string {
      return this.name
   }
}
 
let b = new B();
 
console.log(b.getName());
```

```ts
 
视频案例

//1. class 的基本用法 继承 和 类型约束
//2. class 的修饰符 readonly  private protected public
//3. super 原理
//4. 静态方法
//5. get set
interface Options {
    el: string | HTMLElement
}
 
interface VueCls {
    init(): void
    options: Options
}
 
interface Vnode {
    tag: string
    text?: string
    props?: {
        id?: number | string
        key?: number | string | object
    }
    children?: Vnode[]
}
 
class Dom {
    constructor() {
 
    }
 
    private createElement(el: string): HTMLElement {
        return document.createElement(el)
    }
 
    protected setText(el: Element, text: string | null) {
        el.textContent = text;
    }
 
    protected render(createElement: Vnode): HTMLElement {
        const el = this.createElement(createElement.tag)
        if (createElement.children && Array.isArray(createElement.children)) {
            createElement.children.forEach(item => {
                const child = this.render(item)
                this.setText(child, item.text ?? null)
                el.appendChild(child)
            })
        } else {
            this.setText(el, createElement.text ?? null)
        }
        return el;
    }
}
 
 
 
class Vue extends Dom implements VueCls {
    options: Options
    constructor(options: Options) {
        super()
        this.options = options;
        this.init()
    }
 
   static version () {
      return '1.0.0'
   }
 
   public init() {
        let app = typeof this.options.el == 'string' ? document.querySelector(this.options.el) : this.options.el;
        let data: Vnode = {
            tag: "div",
            props: {
                id: 1,
                key: 1
            },
            children: [
                {
                    tag: "div",
                    text: "子集1",
                },
                {
                    tag: "div",
                    text: "子集2"
                }
            ]
        }
        app?.appendChild(this.render(data))
        console.log(app);
 
        this.mount(app as Element)
    }
 
   public mount(app: Element) {
        document.body.append(app)
    }
}
 
 
const v = new Vue({
    el: "#app"
})
```

