---
title: location
description: 
published: true
date: 2023-03-06T04:02:39.188Z
tags: 
editor: markdown
dateCreated: 2023-02-26T01:31:35.469Z
---

### location对象

window对象给我们提供了一个location属性用于获取或设置窗体的URL，并且可以用于解析URL。因为这个属性返回的是一个对象，所以我们将这个属性也称为location对象。

### **URL**

统一资源定位符(Uniform Resource Locator,URL)是互联网上标准资源的地址。互联网上的每个文件都有一个唯一的URL，它包含的信息指出文件的位置以及浏览器应该怎么处理它。 URL的一般语法格式为:

`protocol:// host [:port]/path/ [?query]#fragment http : / / www.itcast.cn/index.html ?name=andy&age=18#link`

| 组成 说明 |                                                                        |
| ----------- | ------------------------------------------------------------------------ |
| protocol  | 通信协议常用的http,ftp,maito等                                         |
| host      | 主机(域名)[www.itheima.com](http://www.itheima.com)                                                             |
| port      | 端☐号可选,省略时使用方案的默认端☐如http的默认端☐为80                |
| path      | 路径由零或多个/符号隔开的字符串,一般用来表示主机上的一个目录或文件地址 |
| query     | 参数以键值对的形式,通过&符号分隔开来                                   |
| fragment  | 片段#后面内容常见于链接锚点                                            |

### **location 对象的属性**

| location对象属性  | 返回值                          |
| ------------------- | --------------------------------- |
| location.href     | 获取或者设置整个URL             |
| location. host    | 返回主机(域名)[www.itheima.com](http://www.itheima.com)                  |
| location.port     | 返回端☐号如果未写返回空字符串  |
| location.pathname | 返回路径                        |
| location. search  | 返回参数                        |
| location. hash    | 返回片段#后面内容常见于链接锚点 |

**重点记住: ​**​**`href`**​**和**​**`search`**

### **location对象的常见方法**

| location对象方法   | 返回值                                                          |
| -------------------- | ----------------------------------------------------------------- |
| location.assign()  | 跟href一样,可以跳转页面(也称为重定向页面)                       |
| location.replace() | 替换当前页面,因为不记录历史,所以不能后退页面                    |
| location.reload()  | 重新加载页面,相当于刷新按钮或者f5如果参数为true 强制刷新ctrl+f5 |

`<button>点击</button> <script>     var btn = document.querySelector('button');     btn.addEventListener('click', function() {         // 记录浏览历史，所以可以实现后退功能         // location.assign('<http://www.itcast.cn>');         // 不记录浏览历史，所以不可以实现后退功能         // location.replace('<http://www.itcast.cn>');         location.reload(true);     }) </script>`