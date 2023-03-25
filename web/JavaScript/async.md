---
title: 异步编程
description: 
published: true
date: 2023-03-06T03:15:15.025Z
tags: 
editor: markdown
dateCreated: 2023-02-25T11:35:14.972Z
---

# 

### 同步任务和异步任务

单线程导致的问题就是后面的任务等待前面任务完成，如果前面任务很耗时（比如读取网络数据），后面任务不得不一直等待！！

为了解决这个问题，利用多核 CPU 的计算能力，HTML5 提出 Web Worker 标准，允许 JavaScript 脚本创建多个线程，但是子线程完全受主线程控制。于是，JS 中出现了**同步任务**和**异步任务**。

### **同步**

前一个任务结束后再执行后一个任务，程序的执行顺序与任务的排列顺序是一致的、同步的。比如做饭的同步做法：我们要烧水煮饭，等水开了（10分钟之后），再去切菜，炒菜。

### **异步**

你在做一件事情时，因为这件事情会花费很长时间，在做这件事的同时，你还可以去处理其他事情。比如做饭的异步做法，我们在烧水的同时，利用这10分钟，去切菜，炒菜。

**他们的本质区别:这条流水线上各个流程的执行顺序不同。**

> JS中所有任务可以分成两种，一种是同步任务（synchronous），另一种是异步任务（asynchronous）。
>
> 同步任务指的是： 在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务； 异步任务指的是： 不进入主线程、而进入”任务队列”的任务，当主线程中的任务运行完了，才会从”任务队列”取出异步任务放入主线程执行。

### **同步任务**

同步任务都在主线程上执行，形成一个执行栈。

### 异步任务

![https://cloutp.github.io/wiki/web/JavaScript/images/image-20220328110941989.png](https://cloutp.github.io/wiki/web/JavaScript/images/image-20220328110941989.png)

JS的异步是通过回调函数实现的。 —般而言，异步任务有以下三种类型:

1. 普通事件，如click、resize等
2. 资源加载，如load、error等
3. 定时器，包括setlnterval、setTimeout等

**异步任务相关回调函数添加到任务队列中（任务队列也称为消息队列)。**

---

### JS执行机制（事件循环）

![https://cloutp.github.io/web/JavaScript/images/image-20220328111036482.png](https://cloutp.github.io/web/JavaScript/images/image-20220328111036482.png)

1. 先执行执行栈中的同步任务。
2. 异步任务(回调函数)放入任务队列中。
3. 一旦执行栈中的所有同步任务执行完毕，系统就会按次序读取任务队列中的异步任务，于是被读取的异步任务结束等待状态，进入执行栈，开始执行。

![https://cloutp.github.io/web/JavaScript/images/1551435398306.png](https://cloutp.github.io/web/JavaScript/images/1551435398306.png)

由于主线程不断的重复获得任务、执行任务、再获取任务、再执行，所以这种机制被称为**事件循环（ event loop).**