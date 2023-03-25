---
title: ajax
description: 
published: true
date: 2023-03-06T06:08:53.859Z
tags: 
editor: markdown
dateCreated: 2023-02-26T01:45:42.852Z
---

# AJAX

> Ajax：标准读音 [ˈeɪˌdʒæks] ，中文音译：阿贾克斯
>
> 实现页面无刷新更新数据，提高用户浏览网站应用的体验。
>
> Ajax 技术需要运行在网站环境中才能生效，后续会使用Node创建的服务器作为网站服务器。

### 应用场景

1. 页面上拉加载更多数据
2. 列表数据无刷新分页
3. 表单项离开焦点数据验证
4. 搜索框提示文字下拉列表

### Ajax运行原理

> Ajax 相当于浏览器发送请求与接收响应的代理人，以实现在不影响用户浏览页面的情况下，局部更新页面数据，从而提高用户体验。

![https://raw.githubusercontent.com/cloutp/blog_img/main/202201281215546.png](https://raw.githubusercontent.com/cloutp/blog_img/main/202201281215546.png)


### 封装

> 发送一次请求代码过多，发送多次请求代码冗余且重复。将请求代码封装到函数中，发请求时调用函数即可

`ajax({       type: 'get',      url: '<http://www.example.com>',      success: function (data) {           console.log(data);      }  })`

`function ajax (options) {             // 存储的是默认值             var defaults = {                 type: 'get',                 url: '',                 data: {},                 header: {                     'Content-Type': 'application/x-www-form-urlencoded'                 },                 success: function () {},                 error: function () {}             };             // 使用options对象中的属性覆盖defaults对象中的属性             Object.assign(defaults, options);             // 创建ajax对象             var xhr = new XMLHttpRequest();             // 拼接请求参数的变量             var params = '';             // 循环用户传递进来的对象格式参数             for (var attr in defaults.data) {                 // 将参数转换为字符串格式                 params += attr + '=' + defaults.data[attr] + '&';             }             // 将参数最后面的&截取掉              // 将截取的结果重新赋值给params变量             params = params.substr(0, params.length - 1);             // 判断请求方式             if (defaults.type == 'get') {                 defaults.url = defaults.url + '?' + params;             }             /*                 {                     name: 'zhangsan',                     age: 20                 }                 name=zhangsan&age=20              */             // 配置ajax对象             xhr.open(defaults.type, defaults.url);             // 如果请求方式为post             if (defaults.type == 'post') {                 // 用户希望的向服务器端传递的请求参数的类型                 var contentType = defaults.header['Content-Type']                 // 设置请求参数格式的类型                 xhr.setRequestHeader('Content-Type', contentType);                 // 判断用户希望的请求参数格式的类型                 // 如果类型为json                 if (contentType == 'application/json') {                     // 向服务器端传递json数据格式的参数                     xhr.send(JSON.stringify(defaults.data))                 }else {                     // 向服务器端传递普通类型的请求参数                     xhr.send(params);                 }             }else {                 // 发送请求                 xhr.send();             }             // 监听xhr对象下面的onload事件             // 当xhr对象接收完响应数据后触发             xhr.onload = function () {                 // xhr.getResponseHeader()                 // 获取响应头中的数据                 var contentType = xhr.getResponseHeader('Content-Type');                 // 服务器端返回的数据                 var responseText = xhr.responseText;                 // 如果响应类型中包含applicaition/json                 if (contentType.includes('application/json')) {                     // 将json字符串转换为json对象                     responseText = JSON.parse(responseText)                 }                 // 当http状态码等于200的时候                 if (xhr.status == 200) {                     // 请求成功 调用处理成功情况的函数                     defaults.success(responseText, xhr);                 }else {                     // 请求失败 调用处理失败情况的函数                     defaults.error(responseText, xhr);                 }             }         }`



### 详细
- [使用](/web/ajax/使用)
- [状态码](/web/ajax/状态码)
- [错误处理](/web/ajax/错误处理)
- [同源政策](/web/ajax/同源政策)
- [formdata](/web/ajax/formdata)

