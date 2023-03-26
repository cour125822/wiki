---
title: bcrypt
description: 
published: true
date: 2023-03-26T08:10:06.243Z
tags: 
editor: markdown
dateCreated: 2023-02-26T02:26:36.863Z
---

### 密码加密 bcrypt

`// 导入bcrypt模块 const bcrypt = require('bcrypt'); // 生成随机字符串 gen => generate 生成 salt 盐 let salt = await bcrypt.genSalt(10); // 使用随机字符串对密码进行加密 let pass = await bcrypt.hash('明文密码', salt);`

`// 密码比对 let isEqual = await bcrypt.compare('明文密码', '加密密码');`

bcrypt依赖的其他环境

1. python 2.x
2. node-gyp

`npm install -g node-gyp`

1. windows-build-tools

`npm install --global --production windows-build-tools`