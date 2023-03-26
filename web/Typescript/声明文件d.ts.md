---
title: 声明文件d.ts
description: 
published: true
date: 2023-03-26T15:33:27.482Z
tags: 
editor: markdown
dateCreated: 2023-03-26T15:33:27.482Z
---

# 声明文件 declare  
当使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能。

```ts
declare var 声明全局变量
declare function 声明全局方法
declare class 声明全局类
declare enum 声明全局枚举类型
declare namespace 声明（含有子属性的）全局对象
interface 和 type 声明全局类型
/// <reference /> 三斜线指令
```
例如我们有一个express 和 axios
![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303261534052.png)



 发现express 报错了

让我们去下载他的声明文件

```shell
npm install @types/node -D
```

那为什么axios 没有报错

我们可以去node_modules 下面去找axios 的package json
![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303261534292.png)



发现axios已经指定了声明文件 所以没有报错可以直接用

通过语法declare 暴露我们声明的axios 对象

```ts
declare  const axios: AxiosStatic;
```

如果有一些第三方包确实没有声明文件我们可以自己去定义

名称.d.ts 创建一个文件去声明

案例手写声明文件
index.ts
```ts
import express from 'express'
 
 
const app = express()
 
const router = express.Router()
 
app.use('/api', router)
 
router.get('/list', (req, res) => {
    res.json({
        code: 200
    })
})
 
app.listen(9001,()=>{
    console.log(9001)
})
```
express.d.ts
```ts
declare module 'express' {
    interface Router {
        get(path: string, cb: (req: any, res: any) => void): void
    }
    interface App {
 
        use(path: string, router: any): void
        listen(port: number, cb?: () => void): void
    }
    interface Express {
        (): App
        Router(): Router
 
    }
    const express: Express
    export default express
}
```
关于这些
第三发的声明文件包都收录到了 [npm](https://www.npmjs.com/~types?activeTab=packages)