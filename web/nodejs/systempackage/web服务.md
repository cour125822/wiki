---
title: web服务
description: 
published: true
date: 2023-03-06T04:22:25.914Z
tags: 
editor: markdown
dateCreated: 2023-02-26T02:15:03.974Z
---

## HTTP模块

### 创建web服务器

`// 引用系统模块  const http = require('http');   // 创建web服务器  const app = http.createServer();   // 当客户端发送请求的时候  app.on('request', (req, res) => {         //  响应        res.end('<h1>hi, user</h1>');  });   // 监听3000端口  app.listen(3000);  console.log('服务器已启动，监听3000端口，请访问 localhost:3000')`

### 请求与响应处理

### **请求报文处理**

`app.on('request', (req, res) => {      req.headers  // 获取请求报文      req.url      // 获取请求地址      req.method   // 获取请求方法  });`

### **设置响应报头**

**HTTP状态码**

1. 200 请求成功
2. 404 请求的资源没有被找到
3. 500 服务器端错误
4. 400 客户端请求有语法错误

**内容类型**

1. text/html
2. text/css
3. application/javascript
4. image/jpeg
5. application/json

`app.on('request', (req, res) => {      // 设置响应报文      res.writeHead(200, {          'Content-Type': 'text/html;charset=utf8‘      });  });`

### **GET请求参数**

> 参数被放置在浏览器地址栏中，例如：[http://localhost:3000/?name=zhangsan&amp;age=20](http://localhost:3000/?name=zhangsan&age=20)

**参数获取需要借助系统模块url，url模块用来处理url地址**

`const http = require('http');  // 导入url系统模块 用于处理url地址  const url = require('url');  const app = http.createServer();  app.on('request', (req, res) => {      // 将url路径的各个部分解析出来并返回对象          // true 代表将参数解析为对象格式      let {query} = url.parse(req.url, true);      console.log(query);  });  app.listen(3000);`

### **POST请求参数**

> 参数被放置在请求体中进行传输

**获取POST参数需要使用data事件和end事件**

**使用querystring系统模块将参数转换为对象格式**

`// 导入系统模块querystring 用于将HTTP参数转换为对象格式  const querystring = require('querystring');  app.on('request', (req, res) => {      let postData = '';      // 监听参数传输事件      req.on('data', (chunk) => postData += chunk;);      // 监听参数传输完毕事件      req.on('end', () => {           console.log(querystring.parse(postData));       });   });`

### 路由

> 路由是指客户端请求地址与服务器端程序代码的对应关系。

![https://raw.githubusercontent.com/cloutp/blog_img/main/202201271831296.png](https://raw.githubusercontent.com/cloutp/blog_img/main/202201271831296.png)

**处理：**

`// 当客户端发来请求的时候  app.on('request', (req, res) => {      // 获取客户端的请求路径      let { pathname } = url.parse(req.url);      if (pathname == '/' || pathname == '/index') {          res.end('欢迎来到首页');      } else if (pathname == '/list') {          res.end('欢迎来到列表页页');      } else {         res.end('抱歉, 您访问的页面出游了');      }  });`

### 静态资源

> 服务器端不需要处理，可以直接响应给客户端的资源就是静态资源，例如CSS、JavaScript、image文件。

### 动态资源

> 相同的请求地址不同的响应资源，这种资源就是动态资源。