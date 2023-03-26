---
title: 中间件
description: 
published: true
date: 2023-03-26T08:03:59.808Z
tags: 
editor: markdown
dateCreated: 2023-02-26T08:30:41.176Z
---

# **前戏**

在前面的博客中已经学会了给视图函数加装饰器来判断是用户是否登录，把没有登录的用户请求跳转到登录页面。我们通过给几个特定视图函数加装饰器实现了这个需求。但是以后添加的视图函数可能也需要加上装饰器，这样是不是稍微有点繁琐。

下面的内容即将让我们通过一些特点的方式实现控制全局的效果，运筹帷幄于室内(室外太热~~~)

# **中间件**

## **中间件介绍**

### **什么是中间件？**

官方的说法：中间件是一个用来处理Django的请求和响应的框架级别的钩子。它是一个轻量、低级别的插件系统，用于在全局范围内改变Django的输入和输出。每个中间件组件都负责做一些特定的功能。

但是由于其影响的是全局，所以需要谨慎使用，使用不当会影响性能。

说的直白一点中间件是帮助我们在视图函数执行之前和执行之后都可以做一些额外的操作，它本质上就是一个自定义类，类中定义了几个方法，Django框架会在请求的特定的时间去执行这些方法。

我们一直都在使用中间件，只是没有注意到而已，打开Django项目的Settings.py文件，看到下图的MIDDLEWARE配置项。

```
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

MIDDLEWARE配置项是一个列表(列表是有序的，记住这一点，后面你就知道为什么要强调有序二字)，列表中是一个个字符串，这些字符串其实是一个个类，也就是一个个中间件。

我们之前已经接触过一个csrf相关的中间件了？我们一开始让大家把他注释掉，再提交post请求的时候，就不会被forbidden了，后来学会使用csrf_token之后就不再注释这个中间件了。

那接下来就学习中间件中的方法以及这些方法什么时候被执行。

**img**

## **自定义中间件**

中间件可以定义五个方法，分别是：（主要的是process_request和process_response）

* process_request(self,request)
* process_view(self, request, view_func, view_args, view_kwargs)
* process_template_response(self,request,response)
* process_exception(self, request, exception)
* process_response(self, request, response)

以上方法的返回值可以是None或一个HttpResponse对象，如果是None，则继续按照django定义的规则向后继续执行，如果是HttpResponse对象，则直接将该对象返回给用户。

### **自定义一个中间件示例**

**img**

**img**

### **process_request**

process_request有一个参数，就是request，这个request和视图函数中的request是一样的(在交给Django后面的路由之前，对这个request对象可以进行一系列的操作)。

由于request对象是一样的，所以我们可以对request对象进行一系列的操作，包括request.变量名=变量值,这样的操作，我们可以在后续的视图函数中通过相同的方式即可获取到我们在中间件中设置的值。

它的返回值可以是None也可以是HttpResponse对象。返回值是None的话，按正常流程继续走，交给下一个中间件处理，如果是HttpResponse对象，Django将不执行视图函数，而将相应对象返回给浏览器。

我们来看看多个中间件时，Django是如何执行其中的process_request方法的。

```
from django.utils.deprecation import MiddlewareMixin

class MD1(MiddlewareMixin):

    def process_request(self, request):
        print("MD1里面的 process_request")

class MD2(MiddlewareMixin):
    def process_request(self, request):
        print("MD2里面的 process_request")
        pass
```

在settings.py的MIDDLEWARE配置项中注册上述两个自定义中间件：

```
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'middlewares.MD1',  # 自定义中间件MD1
    'middlewares.MD2'  # 自定义中间件MD2
]
```

此时，我们访问一个视图，会发现终端中打印如下内容：

```
MD1里面的 process_request
MD2里面的 process_request
app01 中的 index视图
```

把MD1和MD2的位置调换一下，再访问一个视图，会发现终端中打印的内容如下：

```
MD2里面的 process_request
MD1里面的 process_request
app01 中的 index视图
```

看结果我们知道：视图函数还是最后执行的，MD2比MD1先执行自己的process_request方法。

在打印一下两个自定义中间件中process_request方法中的request参数，会发现它们是同一个对象。

**由此总结一下：**

1. **中间件的process_request方法是在执行视图函数之前执行的。**
2. **当配置多个中间件时，会按照MIDDLEWARE中的注册顺序，也就是列表的索引值，从前到后依次执行的。**
3. **不同中间件之间传递的request都是同一个对象**

### **process_response**

多个中间件中的process_response方法是按照MIDDLEWARE中的注册顺序**倒序**执行的，也就是说第一个中间件的process_request方法首先执行，而它的process_response方法最后执行，最后一个中间件的process_request方法最后一个执行，它的process_response方法是最先执行。

定义process_response方法时，必须给方法传入两个形参，request和response。request就是上述例子中一样的对象，response是视图函数返回的HttpResponse对象(也就是说这是Django后台处理完之后给出一个的一个具体的视图)。该方法的返回值(必须要有返回值)也必须是HttpResponse对象。如果不返回response而返回其他对象，则浏览器不会拿到Django后台给他的视图，而是我的中间件中返回的对象

```
from django.utils.deprecation import MiddlewareMixin

class MD1(MiddlewareMixin):

    def process_request(self, request):
        print("MD1里面的 process_request")

    def process_response(self, request, response):
        print("MD1里面的 process_response")
        return response

class MD2(MiddlewareMixin):
    def process_request(self, request):
        print("MD2里面的 process_request")
        pass

    def process_response(self, request, response):
        print("MD2里面的 process_response")
        return response
```

访问一个视图，看一下终端的输出：

```
MD2里面的 process_request
MD1里面的 process_request
app01 中的 index视图
MD1里面的 process_response
MD2里面的 process_response
```

看结果可知：

process_response方法是在视图函数之后执行的，并且顺序是MD1比MD2先执行。(此时settings.py中 MD2比MD1先注册)

多个中间件中的process_response方法是按照MIDDLEWARE中的注册顺序**倒序**执行的，也就是说第一个中间件的process_request方法首先执行，而它的process_response方法最后执行，最后一个中间件的process_request方法最后一个执行，它的process_response方法是最先执行。

### **process_view**

process_view(self, request, view_func, view_args, view_kwargs)

该方法有四个参数

request是HttpRequest对象。

view_func是Django即将使用的视图函数。 （它是实际的函数对象，而不是函数的名称作为字符串。）

view_args是将传递给视图的位置参数的列表.

view_kwargs是将传递给视图的关键字参数的字典。 view_args和view_kwargs都不包含第一个视图参数（request）。

Django会在调用视图函数之前调用process_view方法。

它应该返回None或一个HttpResponse对象。 如果返回None，Django将继续处理这个请求，执行任何其他中间件的process_view方法，然后在执行相应的视图。 如果它返回一个HttpResponse对象，那么将不会执行Django的视图函数，而是直接在中间件中掉头，倒叙执行一个个process_response方法，最后返回给浏览器

给MD1和MD2添加process_view方法:

```
from django.utils.deprecation import MiddlewareMixin

class MD1(MiddlewareMixin):

    def process_request(self, request):
        print("MD1里面的 process_request")

    def process_response(self, request, response):
        print("MD1里面的 process_response")
        return response

    def process_view(self, request, view_func, view_args, view_kwargs):
        print("-" * 80)
        print("MD1 中的process_view")
        print(view_func, view_func.__name__)

class MD2(MiddlewareMixin):
    def process_request(self, request):
        print("MD2里面的 process_request")
        pass

    def process_response(self, request, response):
        print("MD2里面的 process_response")
        return response

    def process_view(self, request, view_func, view_args, view_kwargs):
        print("-" * 80)
        print("MD2 中的process_view")
        print(view_func, view_func.__name__)
```

访问index视图函数，看一下输出结果：

```
MD2里面的 process_request
MD1里面的 process_request
--------------------------------------------------------------------------------
MD2 中的process_view
<function index at 0x000001DE68317488> index
--------------------------------------------------------------------------------
MD1 中的process_view
<function index at 0x000001DE68317488> index
app01 中的 index视图
MD1里面的 process_response
MD2里面的 process_response
```

process_view方法是在Django路由系统之后，视图系统之前执行的，执行顺序按照MIDDLEWARE中的注册顺序**从前到后顺序**执行的

### **process_exception**

process_exception(self, request, exception)

该方法两个参数:

一个HttpRequest对象

一个exception是视图函数异常产生的Exception对象。

这个方法只有在视图函数中出现异常了才执行，它返回的值可以是一个None也可以是一个HttpResponse对象。如果是HttpResponse对象，Django将调用模板和中间件中的process_response方法，并返回给浏览器，否则将默认处理异常。如果返回一个None，则交给下一个中间件的process_exception方法来处理异常。它的执行顺序也是按照中间件注册顺序的倒序执行。

给MD1和MD2添加上这个方法：

```
from django.utils.deprecation import MiddlewareMixin

class MD1(MiddlewareMixin):

    def process_request(self, request):
        print("MD1里面的 process_request")

    def process_response(self, request, response):
        print("MD1里面的 process_response")
        return response

    def process_view(self, request, view_func, view_args, view_kwargs):
        print("-" * 80)
        print("MD1 中的process_view")
        print(view_func, view_func.__name__)

    def process_exception(self, request, exception):
        print(exception)
        print("MD1 中的process_exception")

class MD2(MiddlewareMixin):
    def process_request(self, request):
        print("MD2里面的 process_request")
        pass

    def process_response(self, request, response):
        print("MD2里面的 process_response")
        return response

    def process_view(self, request, view_func, view_args, view_kwargs):
        print("-" * 80)
        print("MD2 中的process_view")
        print(view_func, view_func.__name__)

    def process_exception(self, request, exception):
        print(exception)
        print("MD2 中的process_exception")
```

如果视图函数中无异常，process_exception方法不执行。

想办法，在视图函数中抛出一个异常：

```
def index(request):
    print("app01 中的 index视图")
    raise ValueError("呵呵")
    return HttpResponse("O98K")
```

在MD1的process_exception中返回一个响应对象：

```
class MD1(MiddlewareMixin):

    def process_request(self, request):
        print("MD1里面的 process_request")

    def process_response(self, request, response):
        print("MD1里面的 process_response")
        return response

    def process_view(self, request, view_func, view_args, view_kwargs):
        print("-" * 80)
        print("MD1 中的process_view")
        print(view_func, view_func.__name__)

    def process_exception(self, request, exception):
        print(exception)
        print("MD1 中的process_exception")
        return HttpResponse(str(exception))  # 返回一个响应对象
```

看输出结果：

```
MD2里面的 process_request
MD1里面的 process_request
--------------------------------------------------------------------------------
MD2 中的process_view
<function index at 0x0000022C09727488> index
--------------------------------------------------------------------------------
MD1 中的process_view
<function index at 0x0000022C09727488> index
app01 中的 index视图
呵呵
MD1 中的process_exception
MD1里面的 process_response
MD2里面的 process_response
```

注意，这里并没有执行MD2的process_exception方法，因为MD1中的process_exception方法直接返回了一个响应对象。

### **process_template_response（用的比较少）**

process_template_response(self, request, response)

它的参数，一个HttpRequest对象，response是TemplateResponse对象（由视图函数或者中间件产生）。

process_template_response是在视图函数执行完成后立即执行，但是它有一个前提条件，那就是视图函数返回的对象有一个render()方法（或者表明该对象是一个TemplateResponse对象或等价方法）。

```
class MD1(MiddlewareMixin):

    def process_request(self, request):
        print("MD1里面的 process_request")

    def process_response(self, request, response):
        print("MD1里面的 process_response")
        return response

    def process_view(self, request, view_func, view_args, view_kwargs):
        print("-" * 80)
        print("MD1 中的process_view")
        print(view_func, view_func.__name__)

    def process_exception(self, request, exception):
        print(exception)
        print("MD1 中的process_exception")
        return HttpResponse(str(exception))

    def process_template_response(self, request, response):
        print("MD1 中的process_template_response")
        return response

class MD2(MiddlewareMixin):
    def process_request(self, request):
        print("MD2里面的 process_request")
        pass

    def process_response(self, request, response):
        print("MD2里面的 process_response")
        return response

    def process_view(self, request, view_func, view_args, view_kwargs):
        print("-" * 80)
        print("MD2 中的process_view")
        print(view_func, view_func.__name__)

    def process_exception(self, request, exception):
        print(exception)
        print("MD2 中的process_exception")

    def process_template_response(self, request, response):
        print("MD2 中的process_template_response")
        return response
```

views.py中：

```
def index(request):
    print("app01 中的 index视图")

    def render():
        print("in index/render")
        return HttpResponse("O98K")
    rep = HttpResponse("OK")
    rep.render = render
    return rep
```

访问index视图，终端输出的结果：

```
MD2里面的 process_request
MD1里面的 process_request
--------------------------------------------------------------------------------
MD2 中的process_view
<function index at 0x000001C111B97488> index
--------------------------------------------------------------------------------
MD1 中的process_view
<function index at 0x000001C111B97488> index
app01 中的 index视图
MD1 中的process_template_response
MD2 中的process_template_response
in index/render
MD1里面的 process_response
MD2里面的 process_response
```

从结果看出：

视图函数执行完之后，立即执行了中间件的process_template_response方法，顺序是倒序，先执行MD1的，在执行MD2的，接着执行了视图函数返回的HttpResponse对象的render方法，返回了一个新的HttpResponse对象，接着执行中间件的process_response方法。

## **中间件的执行流程**

上一部分，我们了解了中间件中的5个方法，它们的参数、返回值以及什么时候执行，现在总结一下中间件的执行流程。

请求到达中间件之后，先按照正序执行每个注册中间件的process_request方法，process_request方法返回的值是None，就依次执行，如果返回的值是HttpResponse对象，不再执行后面的process_request方法，而是执行当前对应中间件的process_response方法(注意不是掉头执行所有的process_response方法)，将HttpResponse对象返回给浏览器。也就是说：如果MIDDLEWARE中注册了6个中间件，执行过程中，第3个中间件返回了一个HttpResponse对象，那么第4,5,6中间件的process_request和process_response方法都不执行，顺序执行3,2,1中间件的process_response方法。

**img**

process_request方法都执行完后，匹配路由，找到要执行的视图函数，先不执行视图函数，先执行中间件中的process_view方法，process_view方法返回None，继续按顺序执行，所有process_view方法执行完后执行视图函数。假如中间件3 的process_view方法返回了HttpResponse对象，则4,5,6的process_view以及视图函数都不执行，直接从最后一个中间件，也就是中间件6的process_response方法开始倒序执行。

**img**

process_template_response和process_exception两个方法的触发是有条件的，执行顺序也是倒序。总结所有的执行流程如下：

**img**

**img**

## **中间件版登录验证**

中间件版的登录验证需要依靠session，所以数据库中要有django_session表。

[urls.py](http://urls.py)

```
from app02 import views as v2
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^login/',v2.login),
    url(r'^home/',v2.home),
    url(r'^index/',v2.index)
]
```

[views.py](http://views.py)

```
from django.shortcuts import render,redirect,HttpResponse
from app02 import models
# Create your views here.
def login(request):
    error_msg=''
    if request.method=='POST':
        username=request.POST.get('username')
        password=request.POST.get('password')
        user_obj=models.User.objects.filter(username=username,password=password)
        if  user_obj:
            #设置session
            request.session['login']='ok'
            #获取用户想直接访问的URL
            url=request.GET.get('next')
            #如果有，就跳转到客户初始想访问的URL
            if not url:
                #没有则默认跳转到home页面
                url='/home/'
            return redirect(url)
        else:
            error_msg='username or password error!'
    return render(request,'login.html',{'error_msg':error_msg})

def home(request):
    return HttpResponse('<h1>这是home页面 只有登录了才能看到</h1>')

def index(request):
    return HttpResponse('<h1>这是index页面 也只有登录了才能看到<h1>')
```

login.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登陆页面</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
</head>
<body>
<form action="" method="post">
    {% csrf_token %}
    <label for="">username:<input type="text" name="username"></label>
    <label for="">password:<input type="password" name="password"></label>
    <input type="submit" value="submit">
</form>
<h1 style="color: red">{{ error_msg }}</h1>

</body>
</html>
```

[middlewares.py](http://middlewares.py)

```
from django.utils.deprecation import MiddlewareMixin
from django.shortcuts import redirect

class Check_Login(MiddlewareMixin):
    def process_request(self,request):
        next_url=request.path_info
        if not next_url.startswith('/login/'):
            is_login=request.session.get('login','')
            if not is_login:
                return redirect('/login/?next={}'.format(next_url))
```

在settings.py中注册

```
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'middleware.my_middleware.Check_Login',

]
```

附：Django请求流程图

**img**