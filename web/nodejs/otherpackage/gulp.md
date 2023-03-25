---
title: Gulp
description: 
published: true
date: 2023-03-06T04:24:11.721Z
tags: 
editor: markdown
dateCreated: 2023-02-26T02:23:08.866Z
---

### Gulp

> 基于node平台开发的前端构建工具，将机械化操作编写成任务, 想要执行机械化操作时执行一个命令行命令任务就能自动执行了，用机器代替手工，提高开发效率。

### **Gulp使用**

1. 下载gulp库文件`npm install gulp`
2. 在项目根目录下建立`gulpfile.js`文件
3. 重构项目的文件夹结构`src`目录放置源代码文件 `dist`目录放置构建后文件
4. 在`gulpfile.js`文件中编写任务.
5. 在命令行工具中执行gulp任务

### **Gulp提供的方法**

1. gulp.src()：获取任务要处理的文件
2. gulp.dest()：输出文件
3. gulp.task()：建立gulp任务
4. gulp.watch()：监控文件的变化

`const gulp = require('gulp');   // 使用gulp.task()方法建立任务  gulp.task('first', () => {     // 获取要处理的文件     gulp.src('./src/css/base.css')      // 将处理后的文件输出到dist目录     .pipe(gulp.dest('./dist/css'));  });`

### **Gulp插件**

* gulp-htmlmin ：html文件压缩
* gulp-csso ：压缩css
* gulp-babel ：JavaScript语法转化
* gulp-less: less语法转化
* gulp-uglify ：压缩混淆JavaScript
* gulp-file-include 公共文件包含
* browsersync 浏览器实时同步