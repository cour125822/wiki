---
title: Vue Cli
description: 
published: true
date: 2023-03-26T08:07:33.229Z
tags: 
editor: markdown
dateCreated: 2023-02-26T03:37:38.608Z
---

### 常用命令（webpack）

### **安装vue命令行工具**

`npm install -g @vue/cli

# OR

yarn global add @vue/cli`

### **创建项目**

`vue create 项目名称

# OR

vue ui`

> 第一种会在命令行选择版本信息配置，第二种在浏览器图形化界面配置项目

### **启动项目**

`vue serve`

### **生成静态文件**

`vue build`

### **升级**

`npm update -g @vue/cli

# 或者

yarn global upgrade --latest @vue/cli

# -----------------------------------

用法： upgrade [options] [plugin-name] 选项： -t, --to <version>    升级 <plugin-name> 到指定的版本 -f, --from <version>  跳过本地版本检测，默认插件是从此处指定的版本升级上来 -r, --registry <url>  使用指定的 registry 地址安装依赖 --all                 升级所有的插件 --next                检查插件新版本时，包括 alpha/beta/rc 版本在内 -h, --help            输出帮助内容`