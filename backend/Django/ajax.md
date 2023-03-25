---
title: Ajax
description: 
published: true
date: 2023-03-06T05:54:35.144Z
tags: 
editor: markdown
dateCreated: 2023-02-26T08:27:43.232Z
---

> Ajax具体详细查看 AJAX 页面

# **AJAX请求如何设置csrf_token**

不论是ajax还是谁，只要是向我Django提交post请求的数据，都必须校验csrf_token来防伪跨站请求，那么如何在我的ajax中弄这个csrf_token呢，我又不像form表单那样可以在表单内部通过一句`{% csrf_token %}`就搞定了……

### **方式1**

通过获取隐藏的input标签中的csrfmiddlewaretoken值，放置在data中发送。

```
$.ajax({
  url: "/cookie_ajax/",
  type: "POST",
  data: {
    "username": "Tonny",
    "password": 123456,
    "csrfmiddlewaretoken": $("[name = 'csrfmiddlewaretoken']").val()  // 使用JQuery取出csrfmiddlewaretoken的值，拼接到data中
  },
  success: function (data) {
    console.log(data);
  }
})
```

### **方式2**

通过获取返回的cookie中的字符串 放置在请求头中发送。

注意：需要引入一个jquery.cookie.js插件。

```
$.ajax({
  url: "/cookie_ajax/",
  type: "POST",
  headers: {"X-CSRFToken": $.cookie('csrftoken')},  // 从Cookie取csrf_token，并设置ajax请求头
  data: {"username": "Q1mi", "password": 123456},
  success: function (data) {
    console.log(data);
  }
})
```

### **方式3**

或者用自己写一个getCookie方法：

```
function getCookie(name) {
    var cookieValue = null;
    if (document.cookie && document.cookie !== '') {
        var cookies = document.cookie.split(';');
        for (var i = 0; i < cookies.length; i++) {
            var cookie = jQuery.trim(cookies[i]);
            // Does this cookie string begin with the name we want?
            if (cookie.substring(0, name.length + 1) === (name + '=')) {
                cookieValue = decodeURIComponent(cookie.substring(name.length + 1));
                break;
            }
        }
    }
    return cookieValue;
}
var csrftoken = getCookie('csrftoken');
```

每一次都这么写太麻烦了，可以使用$.ajaxSetup()方法为ajax请求统一设置。

```
function csrfSafeMethod(method) {
  // these HTTP methods do not require CSRF protection
  return (/^(GET|HEAD|OPTIONS|TRACE)$/.test(method));
}

$.ajaxSetup({
  beforeSend: function (xhr, settings) {
    if (!csrfSafeMethod(settings.type) && !this.crossDomain) {
      xhr.setRequestHeader("X-CSRFToken", csrftoken);
    }
  }
});
```

将下面的文件配置到你的Django项目的静态文件中，在html页面上通过导入该文件即可自动帮我们解决ajax提交post数据时校验csrf_token的问题，(导入该配置文件之前，需要先导入jQuery，因为这个配置文件内的内容是基于jQuery来实现的)

更多细节详见：[Djagno官方文档中关于CSRF的内容](https://docs.djangoproject.com/en/1.11/ref/csrf/)