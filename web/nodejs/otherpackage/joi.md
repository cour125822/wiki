---
title: Joi
description: 
published: true
date: 2023-03-06T04:24:27.670Z
tags: 
editor: markdown
dateCreated: 2023-02-26T02:26:59.044Z
---

### **Joi**

> JavaScript对象的规则描述语言和验证器。

`const Joi = require('joi'); const schema = {     username: Joi.string().alphanum().min(3).max(30).required().error(new Error(‘错误信息’)),     password: Joi.string().regex(/^[a-zA-Z0-9]{3,30}$/),     access_token: [Joi.string(), Joi.number()],     birthyear: Joi.number().integer().min(1900).max(2013),     email: Joi.string().email() }; Joi.validate({ username: 'abc', birthyear: 1994 }, schema);`