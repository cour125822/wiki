---
title: package.json文件
description: 
published: true
date: 2023-03-26T08:07:15.937Z
tags: 
editor: markdown
dateCreated: 2023-02-26T02:08:53.413Z
---

## package.json文件

### node_modules文件夹的问题

1. 文件夹以及文件过多过碎，当我们将项目整体拷贝给别人的时候,，传输速度会很慢很慢.
2. 复杂的模块依赖关系需要被记录，确保模块的版本和当前保持一致，否则会导致当前项目运行报错

### package.json文件的作用

项目描述文件，记录了当前项目信息，例如项目名称、版本、作者、github地址、当前项目依赖了哪些第三方模块等。

使用`npm init -y`命令生成。

### **项目依赖**

在项目的开发阶段和线上运营阶段，都需要依赖的第三方包，称为项目依赖

使用npm install 包名命令下载的文件会默认被添加到 package.json 文件的 dependencies 字段中

`{     "dependencies": {         "jquery": "^3.3.1“     }  }`

### **开发依赖**

在项目的开发阶段需要依赖，线上运营阶段不需要依赖的第三方包，称为开发依赖

使用`npm install` 包名 `--save-dev`命令将包添加到`package.json`文件的`devDependencies`字段中

`{     "devDependencies": {         "gulp": "^3.9.1“     }  }`

### package-lock.json文件的作用

锁定包的版本，确保再次下载时不会因为包版本不同而产生问题

加快下载速度，因为该文件中已经记录了项目所依赖第三方包的树状结构和包的下载地址，重新安装时只需下载即可，不需要做额外的工作