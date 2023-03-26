---
title: 解析表单
description: 
published: true
date: 2023-03-26T08:10:12.262Z
tags: 
editor: markdown
dateCreated: 2023-02-26T02:27:35.544Z
---

### formidable

> 解析表单，支持get请求参数，post请求参数、文件上传。

`// 引入formidable模块  const formidable = require('formidable');  // 创建表单解析对象  const form = new formidable.IncomingForm();  // 设置文件上传路径  form.uploadDir = "/my/dir";  // 是否保留表单上传文件的扩展名  form.keepExtensions = false;  // 对表单进行解析  form.parse(req, (err, fields, files) => {      // fields 存储普通请求参数          // files 存储上传的文件信息  });`