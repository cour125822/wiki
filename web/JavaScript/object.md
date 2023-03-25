---
title: 对象
description: 
published: true
date: 2023-03-06T03:15:07.224Z
tags: 
editor: markdown
dateCreated: 2023-02-25T11:30:41.101Z
---

# 对象

在 JavaScript 中，对象是一组无序的相关属性和方法的集合，所有的事物都是对象，例如字符串、数值、数组、函数等。 对象是由属性和方法组成的。

* 属性：事物的特征，在对象中用属性来表示（常用名词）
* 方法：事物的行为，在对象中用方法来表示（常用动词）

### 创建对象的三种方式

* 利用字面量创建对象

### **使用对象字面量创建对象**：

```
就是花括号 { } 里面包含了表达这个具体事物（对象）的属性和方法；{ } 里面采取键值对的形式表示
```

* 键：相当于属性名
* 值：相当于属性值，可以是任意类型的值（数字类型、字符串类型、布尔类型，函数类型等） 代码如下： 上述代码中 star即是创建的对象。

  ```
  var star = {
      name : '45',
      age : 18,
      sex : '男',
      sayHi : function(){
          alert('大家好啊~');
      }
  };
  ```
* 对象的使用
* 对象的属性

  * 对象中存储**具体数据**的 “键值对”中的 “键”称为对象的属性，即对象中存储具体数据的项
* 对象的方法

  * 对象中存储**函数**的 “键值对”中的 “键”称为对象的方法，即对象中存储函数的项
* 访问对象的属性

  * 对象里面的属性调用 : 对象.属性名 ，这个小点 . 就理解为“ 的 ”
  * 对象里面属性的另一种调用方式 : 对象[‘属性名’]，注意方括号里面的属性必须加引号

  示例代码如下：

  ```
  console.log(star.name)     // 调用名字属性
  console.log(star['name'])  // 调用名字属性
  ```
* 调用对象的方法

  * 对象里面的方法调用：对象.方法名() ，注意这个方法名字后面一定加括号

  示例代码如下：

  ```
  star.sayHi();              // 调用 sayHi 方法,注意，一定不要忘记带后面的括号
  ```
* 变量、属性、函数、方法总结 属性是对象的一部分，而变量不是对象的一部分，变量是单独存储数据的容器

  * 变量：单独声明赋值，单独存在
  * 属性：对象里面的变量称为属性，不需要声明，用来描述该对象的特征
  * 方法是对象的一部分，函数不是对象的一部分，函数是单独封装操作的容器
  * 函数：单独存在的，通过“函数名()”的方式就可以调用
  * 方法：对象里面的函数称为方法，方法不需要声明，使用“对象.方法名()”的方式就可以调用，方法用来描述该对象的行为和功能。
* 利用 new Object 创建对象
* 创建空对象 通过内置构造函数Object创建对象，此时andy变量已经保存了创建出来的空对象

  ```
  var andy = new Obect();
  ```
* 给空对象添加属性和方法

  * 通过对象操作属性和方法的方式，来为对象增加属性和方法

  示例代码如下：

  ```
  andy.name = 'pink';
  andy.age = 18;
  andy.sex = '男';
  andy.sayHi = function(){
      alert('大家好啊~');
  }
  ```

  注意：

  * Object() ：第一个字母大写
  * new Object() ：需要 new 关键字
  * 使用的格式：对象.属性 = 值;
* 利用构造函数创建对象
* 构造函数 构造函数，如 Stars()，抽象了对象的公共部分，封装到了函数里面，它泛指某一大类（class） 创建对象，如 new Stars()，特指某一个，通过 new 关键字创建对象的过程我们也称为对象实例化

  * 构造函数：是一种特殊的函数，主要用来初始化对象，即为对象成员变量赋初始值，它总与 new 运算符一起使用。我们可以把对象中一些公共的属性和方法抽取出来，然后封装到这个函数里面。
  * 构造函数的封装格式：

  ```
  function 构造函数名(形参1,形参2,形参3) {
       this.属性名1 = 参数1;
       this.属性名2 = 参数2;
       this.属性名3 = 参数3;
       this.方法名 = 函数体;
  }
  ```

  * 构造函数的调用格式

  ```
  var obj = new 构造函数名(实参1，实参2，实参3)
  ```

  以上代码中，obj即接收到构造函数创建出来的对象。

  * 注意事项
  * 构造函数约定**首字母大写**。
  * 函数内的属性和方法前面需要添加 **this** ，表示当前对象的属性和方法。
  * 构造函数中**不需要 return 返回结果**。
  * 当我们创建对象的时候，**必须用 new 来调用构造函数**。
  * 其他
* new关键字的作用
* 在构造函数代码开始执行之前，创建一个空对象；
* 修改this的指向，把this指向创建出来的空对象；
* 执行函数的代码
* 在函数完成之后，返回this—即创建出来的对象

### 遍历对象

`for...in` 语句用于对数组或者对象的属性进行循环操作。

其语法如下：

```
for (变量 in 对象名字) {
    // 在此执行代码
}
```

语法中的变量是自定义的，它需要符合命名规范，通常我们会将这个变量写为 k 或者 key。

```
for (var k in obj) {
    console.log(k);      // 这里的 k 是属性名
    console.log(obj[k]); // 这里的 obj[k] 是属性值
}
```

## 构造函数和原型

### 对象的三种创建方式–复习

1. 字面量方式

```
var obj = {};
```

1. new关键字

```
var obj = new Object();
```

1. 构造函数方式

```
function Person(name,age){
  this.name = name;
  this.age = age;
}
var obj = new Person('zs',12);
```

### 静态成员和实例成员

### 实例成员

实例成员就是构造函数内部通过this添加的成员 如下列代码中uname age sing 就是实例成员,实例成员只能通过实例化的对象来访问

```
 function Star(uname, age) {
     this.uname = uname;
     this.age = age;
     this.sing = function() {
     console.log('我会唱歌');
    }
}
var ldh = new Star('刘德华', 18);
console.log(ldh.uname);//实例成员只能通过实例化的对象来访问
```

### 静态成员

静态成员 在构造函数本身上添加的成员 如下列代码中 sex 就是静态成员,静态成员只能通过构造函数来访问

```
 function Star(uname, age) {
     this.uname = uname;
     this.age = age;
     this.sing = function() {
     console.log('我会唱歌');
    }
}
Star.sex = '男';
var ldh = new Star('刘德华', 18);
console.log(Star.sex);//静态成员只能通过构造函数来访问
```

### 构造函数的问题

构造函数方法很好用，但是存在浪费内存的问题。

我们希望所有的对象使用同一个函数，这样就比较节省内存

### 构造函数原型prototype

构造函数通过原型分配的函数是所有对象所共享的。

JavaScript 规定，每一个构造函数都有一个prototype 属性，指向另一个对象。注意这个prototype就是一个对象，这个对象的所有属性和方法，都会被构造函数所拥有。

我们可以把那些不变的方法，直接定义在 prototype 对象上，这样所有对象的实例就可以共享这些方法。

```
function Star(uname, age) {
    this.uname = uname;
    this.age = age;
}
Star.prototype.sing = function() {
    console.log('我会唱歌');
}
var ldh = new Star('刘德华', 18);
var zxy = new Star('张学友', 19);
ldh.sing();//我会唱歌
zxy.sing();//我会唱歌
```

### 对象原型

```
对象都会有一个属性 __proto__ 指向构造函数的 prototype 原型对象，之所以我们对象可以使用构造函数 prototype 原型对象的属性和方法，就是因为对象有 __proto__ 原型的存在。
__proto__对象原型和原型对象 prototype 是等价的
__proto__对象原型的意义就在于为对象的查找机制提供一个方向，或者说一条路线，但是它是一个非标准属性，因此实际开发中，不可以使用这个属性，它只是内部指向原型对象 prototype
```

![https://cloutp.github.io/web/JavaScript/images/img2.png](https://cloutp.github.io/web/JavaScript/images/img2.png)

```
function Star( uname, age) {
    this.uname = uname;
    this.age = age;
}
Star. prototype.sing = function() {
    consoLe. 1og('我会唱歌');
}
var ldh = new Star( '刘德华'，18);
var zxy = new Star('张学友'，19);ldh.sing();
console.log( ldh);
consoLe.log(ldh._proto__ === Star.prototype);/ /true
```

### constructor构造函数

```
对象原型（ __proto__）和构造函数（prototype）原型对象里面都有一个属性 constructor 属性 ，constructor 我们称为构造函数，因为它指回构造函数本身。
constructor 主要用于记录该对象引用于哪个构造函数，它可以让原型对象重新指向原来的构造函数。
一般情况下，对象的方法都在构造函数的原型对象中设置。如果有多个对象的方法，我们可以给原型对象采取对象形式赋值，但是这样就会覆盖构造函数原型对象原来的内容，这样修改后的原型对象 constructor  就不再指向当前构造函数了。此时，我们可以在修改后的原型对象中，添加一个 constructor 指向原来的构造函数。
```

如果我们修改了原来的原型对象,给原型对象赋值的是一个对象,则必须手动的利用constructor指回原来的构造函数如:

```
 function Star(uname, age) {
     this.uname = uname;
     this.age = age;
 }
 // 很多情况下,我们需要手动的利用constructor 这个属性指回 原来的构造函数
 Star.prototype = {
 // 如果我们修改了原来的原型对象,给原型对象赋值的是一个对象,则必须手动的利用constructor指回原来的构造函数
   constructor: Star, // 手动设置指回原来的构造函数
   sing: function() {
     console.log('我会唱歌');
   },
   movie: function() {
     console.log('我会演电影');
   }
}
var zxy = new Star('张学友', 19);
console.log(zxy)
```

* 设置constructor属性
* 未设置constructor属性

### 原型链

每一个实例对象又有一个__proto__属性，指向的构造函数的原型对象，构造函数的原型对象也是一个对象，也有__proto__属性，这样一层一层往上找就形成了原型链。

![https://cloutp.github.io/web/JavaScript/images/img5.png](https://cloutp.github.io/web/JavaScript/images/img5.png)

### 构造函数实例和原型对象三角关系

```
1.构造函数的prototype属性指向了构造函数原型对象
2.实例对象是由构造函数创建的,实例对象的__proto__属性指向了构造函数的原型对象
3.构造函数的原型对象的constructor属性指向了构造函数,实例对象的原型的constructor属性也指向了构造函数
```

![https://cloutp.github.io/web/JavaScript/images/img4.png](https://cloutp.github.io/web/JavaScript/images/img4.png)

### 原型链和成员的查找机制

任何对象都有原型对象,也就是prototype属性,任何原型对象也是一个对象,该对象就有__proto__属性,这样一层一层往上找,就形成了一条链,我们称此为原型链;

```
当访问一个对象的属性（包括方法）时，首先查找这个对象自身有没有该属性。
如果没有就查找它的原型（也就是 __proto__指向的 prototype 原型对象）。
如果还没有就查找原型对象的原型（Object的原型对象）。
依此类推一直找到 Object 为止（null）。
__proto__对象原型的意义就在于为对象成员查找机制提供一个方向，或者说一条路线。
```

### 原型对象中this指向

构造函数中的this和原型对象的this,都指向我们new出来的实例对象

```
function Star(uname, age) {
    this.uname = uname;
    this.age = age;
}
var that;
Star.prototype.sing = function() {
    console.log('我会唱歌');
    that = this;
}
var ldh = new Star('刘德华', 18);
// 1. 在构造函数中,里面this指向的是对象实例 ldh
console.log(that === ldh);//true
// 2.原型对象函数里面的this 指向的是 实例对象 ldh
```

### 通过原型为数组扩展内置方法

```
 Array.prototype.sum = function() {
   var sum = 0;
   for (var i = 0; i < this.length; i++) {
   sum += this[i];
   }
   return sum;
 };
 //此时数组对象中已经存在sum()方法了  可以始终 数组.sum()进行数据的求
```

## 继承

### call()

* call()可以调用函数
* call()可以修改this的指向,使用call()的时候 参数一是修改后的this指向,参数2,参数3..使用逗号隔开连接

```
 function fn(x, y) {
     console.log(this);
     console.log(x + y);
}
  var o = {
    name: 'andy'
  };
  fn.call(o, 1, 2);//调用了函数此时的this指向了对象o,
```

### 子构造函数继承父构造函数中的属性

1. 先定义一个父构造函数
2. 再定义一个子构造函数
3. 子构造函数继承父构造函数的属性(使用call方法)

```
 // 1. 父构造函数
 function Father(uname, age) {
   // this 指向父构造函数的对象实例
   this.uname = uname;
   this.age = age;
 }
  // 2 .子构造函数
function Son(uname, age, score) {
  // this 指向子构造函数的对象实例
  3.使用call方式实现子继承父的属性
  Father.call(this, uname, age);
  this.score = score;
}
var son = new Son('刘德华', 18, 100);
console.log(son);
```

### 借用原型对象继承方法

1. 先定义一个父构造函数
2. 再定义一个子构造函数
3. 子构造函数继承父构造函数的属性(使用call方法)

```
// 1. 父构造函数
function Father(uname, age) {
  // this 指向父构造函数的对象实例
  this.uname = uname;
  this.age = age;
}
Father.prototype.money = function() {
  console.log(100000);
 };
 // 2 .子构造函数
  function Son(uname, age, score) {
      // this 指向子构造函数的对象实例
      Father.call(this, uname, age);
      this.score = score;
  }
// Son.prototype = Father.prototype;  这样直接赋值会有问题,如果修改了子原型对象,父原型对象也会跟着一起变化
  Son.prototype = new Father();
  // 如果利用对象的形式修改了原型对象,别忘了利用constructor 指回原来的构造函数
  Son.prototype.constructor = Son;
  // 这个是子构造函数专门的方法
  Son.prototype.exam = function() {
    console.log('孩子要考试');

  }
  var son = new Son('刘德华', 18, 100);
  console.log(son);
```