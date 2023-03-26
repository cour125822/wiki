---
title: drf
description: 
published: true
date: 2023-03-26T08:03:02.341Z
tags: 
editor: markdown
dateCreated: 2023-02-26T08:33:55.241Z
---

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cd75fe4f-8436-40f3-98d3-93a47f7dba12/Untitled.png)

## **简单安装使用**

shell

```
# 安装：
pip install djangorestframework==3.10.3
```

python

```
# 使用
    1 在setting.py 的app中注册
        INSTALLED_APPS = [
        'rest_framework'
        ]
    2 在models.py中写表模型
        class Book(models.Model):
            nid=models.AutoField(primary_key=True)
            name=models.CharField(max_length=32)
            price=models.DecimalField(max_digits=5,decimal_places=2)
            author=models.CharField(max_length=32)
    3 新建一个序列化类（听不懂）
        from rest_framework.serializers import ModelSerializer
        from app01.models import  Book
        class BookModelSerializer(ModelSerializer):
            class Meta:
                model = Book
                fields = "__all__"
    4 在视图中写视图类
        from rest_framework.viewsets import ModelViewSet
        from .models import Book
        from .ser import BookModelSerializer
        class BooksViewSet(ModelViewSet):
            queryset = Book.objects.all()
            serializer_class = BookModelSerializer
    5 写路由关系
        from app01 import views
        from rest_framework.routers import DefaultRouter
        router = DefaultRouter()  # 可以处理视图的路由器
        router.register('book', views.BooksViewSet)  # 向路由器中注册视图集
          # 将路由器中的所以路由信息追到到django的路由列表中
        urlpatterns = [
            path('admin/', admin.site.urls),
        ]
        #这是什么意思？两个列表相加
        # router.urls  列表
        urlpatterns += router.urls

    6 启动，在postman中测试即可
```

## **局部禁用csrf**

python

```
# 在视图函数上加装饰器@csrf_exempt
# csrf_exempt(view)这么写和在视图函数上加装饰器是一毛一样的

#urls.py中看到这种写法
path('tset/', csrf_exempt(views.test)),
```

**[Restful规范](https://www.notion.so/Restful-f237bc50842646a987fc1ee4f6022621)**

**[Serializer](https://www.notion.so/Serializer-501612d858e04385a02a1bf7bc9dbf7f)**

**[请求与响应](https://www.notion.so/5811b8fc72ff4458bd58009b6b748023)**

**[视图](https://www.notion.so/22ed3bd016d74f53a041c5dd91295893)**

**[路由](https://www.notion.so/3b9342f10a464c29875f95a048585f13)**

**[认证权限频率](https://www.notion.so/45d93a27a61a454e839b386fa957aed2)**

**[过滤排序分页异常处理](https://www.notion.so/3a1a47f39ffb421da6d9634246cb862d)**

**[自动生成接口文档](https://www.notion.so/d42df8e207514f04bdf7d5a2f8e6c85e)**

**[JWT认证](https://www.notion.so/JWT-bf563cc20ce54434b1f0a4014423d578)**

**[Xadmin的使用](https://www.notion.so/Xadmin-8fb7333bdfb34998a4063385369138c5)**

**[Book系列多表群操作](https://www.notion.so/Book-c22aaf7885e9461c8fb2d957d7583e3f)**

[RBAC-基于角色的访问控制](https://www.notion.so/RBAC-29e30fe6a5ee4032a71bd4b1308c9168)