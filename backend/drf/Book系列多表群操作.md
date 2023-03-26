---
title: Book系列多表群操作
description: 
published: true
date: 2023-03-26T08:04:25.386Z
tags: 
editor: markdown
dateCreated: 2023-02-26T08:43:23.646Z
---

## **Book系列多表群操作**

2020-07-12

阅读数：253次

# **Book系列连表接口**

## [views.py](http://views.py)****

```
from django.shortcuts import render

# Create your views here.
from rest_framework.views import APIView
from rest_framework.viewsets import ModelViewSet
from app01.models import Book
# from app01.ser import BookSerializers
from rest_framework.decorators import action
from rest_framework.response import Response
from rest_framework.authentication import SessionAuthentication, BasicAuthentication
# class TestView(APIView):
#     def get(self,request):
#         1/0
#         return Response({'msg':'个人中心'})
#
# class BookViewSet(ModelViewSet):
#     authentication_classes = [BasicAuthentication,]
#     queryset = Book.objects.all()
#     serializer_class = BookSerializers
    # @action(methods=['get'], detail=False)
    # def login(self, request):
    #     Book.objects.update_or_create()
    #     return Response({'msg':'登陆成功'})
    # @action(methods=['put'], detail=True)
    # def get_new_5(self, request,pk):
    #     return Response({'msg':'获取5条数据成功'})

from rest_framework.permissions import AllowAny,IsAuthenticated,IsAdminUser,IsAuthenticatedOrReadOnly

from app01.response import APIResponse
from app01 import models
from app01 import ser as serializers
class PublishAPIView(APIView):
    def get(self, request, *args, **kwargs):
        pk = kwargs.get('pk')
        if pk:
            publish_obj = models.Publish.objects.filter(pk=pk).first()
            if not publish_obj:
                return APIResponse(1, 'pk error', http_status=400)
            publish_data = serializers.PublishModelSerializer(publish_obj).data
            return APIResponse(results=publish_data)

        publish_query = models.Publish.objects.all()
        return APIResponse(0, 'ok', data=serializers.PublishModelSerializer(publish_query, many=True).data)

class BookAPIView(APIView):
    # 单查、群查
    def get(self, request, *args, **kwargs):
        pk = kwargs.get('pk')
        if pk:
            book_obj = models.Book.objects.filter(pk=pk, is_delete=False).first()
            if not book_obj:
                return APIResponse(1, 'pk error', http_status=400)
            book_data = serializers.BookModelSerializer(book_obj).data
            print(book_data)
            return APIResponse(data=book_data)

        book_query = models.Book.objects.filter(is_delete=False).all()

        return APIResponse(0, 'ok', data=serializers.BookModelSerializer(book_query, many=True).data)

    # 单删、群删
    def delete(self, request, *args, **kwargs):
        """
        单删：前台数据为pk，接口为 /books/(pk)/
        群删：前台数据为pks，接口为 /books/
        """
        pk = kwargs.get('pk')
        # 将单删群删逻辑整合
        if pk:  # /books/(pk)/的接口就不考虑群删，就固定为单删
            pks = [pk]
        else:
            pks = request.data.get('pks')
        # 前台数据有误(主要是群删没有提供pks)
        if not pks:
            return APIResponse(1, 'delete error', http_status=400)
        # 只要有操作受影响行，就是删除成功，反之失败
        rows = models.Book.objects.filter(is_delete=False, pk__in=pks).update(is_delete=True)
        if rows:
            return APIResponse(0, 'delete ok')
        return APIResponse(1, 'delete failed')

    # 单增、群增
    def post(self, request, *args, **kwargs):
        """
        单增：前台提交字典，接口 /books/
        群增：前台提交列表套字典，接口 /books/
        """
        request_data = request.data
        if isinstance(request_data, dict):  # 单增
            book_ser = serializers.BookModelSerializer(data=request_data)
            if book_ser.is_valid():
                book_obj = book_ser.save()
                return APIResponse(data=serializers.BookModelSerializer(book_obj).data)
            return APIResponse(1, msg=book_ser.errors)
        elif isinstance(request_data, list) and len(request_data) != 0 :  # 群增
            book_ser = serializers.BookModelSerializer(data=request_data, many=True)
            book_ser.is_valid(raise_exception=True)
            book_obj_list = book_ser.save()
            return APIResponse(data=serializers.BookModelSerializer(book_obj_list, many=True).data)
        else:
            return APIResponse(1, 'data error', http_status=400)

    # 单整体改、群整体改
    def put(self, request, *args, **kwargs):
        """
        单整体改：前台提交字典，接口 /books/(pk)/
        群整体改：前台提交列表套字典，接口 /books/，注每一个字典都可以通过pk
        """
        pk = kwargs.get('pk')
        request_data = request.data
        if pk: # 单改
            try:
                book_obj = models.Book.objects.get(pk=pk)
            except:
                return APIResponse(1, 'pk error')

            # 修改和新增，都需要通过数据，数据依旧给data，修改与新增不同点，instance要被赋值为被修改对象
            book_ser = serializers.BookModelSerializer(instance=book_obj, data=request_data)
            book_ser.is_valid(raise_exception=True)
            book_obj = book_ser.save()
            return APIResponse(data=serializers.BookModelSerializer(book_obj).data)
        else:  # 群改
            if not isinstance(request_data, list) or len(request_data) == 0:
                return APIResponse(1, 'data error', http_status=400)

            # [{pk:1,...}, {pk:3,...}, {pk:100,...}] => [obj1, obj3, obj100] + [{...}, {...}, {...}]
            # 要考虑pk对应的对象是否被删，以及pk没有对应的对象
            # 假设pk3被删，pk100没有 => [obj1] + [{...}]

            # 注：一定不要在循环体中对循环对象进行增删(影响对象长度)的操作
            obj_list = []
            data_list = []
            for dic in request_data:
                # request_data可能是list，单内部不一定是dict
                try:
                    pk = dic.pop('pk')
                    try:
                        obj = models.Book.objects.get(pk=pk, is_delete=False)
                        obj_list.append(obj)
                        data_list.append(dic)
                    except:
                        pass
                except:
                    return APIResponse(1, 'data error', http_status=400)

            book_ser = serializers.BookModelSerializer(instance=obj_list, data=data_list, many=True)
            book_ser.is_valid(raise_exception=True)
            book_obj_list = book_ser.save()
            return APIResponse(data=serializers.BookModelSerializer(book_obj_list, many=True).data)

    # 单局部改、群局部改
    def patch(self, request, *args, **kwargs):
        """
        单整体改：前台提交字典，接口 /books/(pk)/
        群整体改：前台提交列表套字典，接口 /books/，注每一个字典都可以通过pk
        """
        pk = kwargs.get('pk')
        request_data = request.data
        if pk:
            try:
                book_obj = models.Book.objects.get(pk=pk)
            except:
                return APIResponse(1, 'pk error')
            # 局部修改就是在整体修改基础上设置partial=True，将所有参与反序列化字段设置为required=False
            book_ser = serializers.BookModelSerializer(instance=book_obj, data=request_data, partial=True)
            book_ser.is_valid(raise_exception=True)
            book_obj = book_ser.save()
            return APIResponse(data=serializers.BookModelSerializer(book_obj).data)

        else:  # 群改
            if not isinstance(request_data, list) or len(request_data) == 0:
                return APIResponse(1, 'data error', http_status=400)

            # [{pk:1,...}, {pk:3,...}, {pk:100,...}] => [obj1, obj3, obj100] + [{...}, {...}, {...}]
            # 要考虑pk对应的对象是否被删，以及pk没有对应的对象
            # 假设pk3被删，pk100没有 => [obj1] + [{...}]

            # 注：一定不要在循环体中对循环对象进行增删(影响对象长度)的操作
            obj_list = []
            data_list = []
            for dic in request_data:
                # request_data可能是list，单内部不一定是dict
                try:
                    pk = dic.pop('pk')
                    try:
                        obj = models.Book.objects.get(pk=pk, is_delete=False)
                        obj_list.append(obj)
                        data_list.append(dic)
                    except:
                        pass
                except:
                    return APIResponse(1, 'data error', http_status=400)

            book_ser = serializers.BookModelSerializer(instance=obj_list, data=data_list, many=True, partial=True)
            book_ser.is_valid(raise_exception=True)
            book_obj_list = book_ser.save()
            return APIResponse(data=serializers.BookModelSerializer(book_obj_list, many=True).data)

class AuthorAPIView(APIView):
    def get(self,request,*args,**kwargs):
        authors=models.Author.objects.all()
        author_ser=serializers.AuthorModelSerializer(authors,many=True)
        return APIResponse(data=author_ser.data)
    def put(self,reuqest,*args,**kwargs):
        pass
    def post(self,request,*args,**kwargs):
        author_ser=serializers.AuthorModelSerializer(data=request.data)
        author_ser.is_valid(raise_exception=True)
        author_ser.save()
        return APIResponse()
    def delete(self,request,*args,**kwargs):
        pass
```

## [ser.py](http://ser.py)****

```
from rest_framework import serializers
from app01 import models
class BookListSerializer(serializers.ListSerializer):
    # 1、create方法父级ListSerializer已经提供了
    # def create(self, validated_data):
    #     # 通过self.child来访问绑定的ModelSerializer
    #     print(self.child)
    #     raise Exception('我不提供')

    # 2、父级ListSerializer没有通过update方法的实现体，需要自己重写
    def update(self, instance, validated_data):
        # print(instance)
        # print(validated_data)
        return [
            self.child.update(instance[i], attrs) for i, attrs in enumerate(validated_data)
        ]

class BookModelSerializer(serializers.ModelSerializer):
    # 通过BookModelSerializer.Meta.list_serializer_class来访问绑定的ListSerializer
    class Meta:
        # 关联ListSerializer完成群增群改
        list_serializer_class = BookListSerializer

        model = models.Book
        # fields = ('name', 'price', 'publish', 'authors')
        # fields = ('name', 'price', 'publish_name', 'author_list')

        # 了解
        # fields = '__all__'
        # exclude = ('id', )
        # depth = 1

        # 序列化与反序列化整合
        fields = ('name', 'price', 'publish_name', 'author_list', 'publish', 'authors')
        extra_kwargs = {
            'publish': {
                'write_only': True
            },
            'authors': {
                'write_only': True
            }
        }

# 前提：如果只有查需求的接口，自定义深度还可以用子序列化方式完成
class PublishModelSerializer(serializers.ModelSerializer):
    # 子序列化都是提供给外键(正向方向)完成深度查询的，外键数据是唯一：many=False；不唯一：many=True
    # 注：只能参与序列化，且反序列化不能写(反序列化外键字段会抛异常)
    books = BookModelSerializer(many=True)
    class Meta:
        model = models.Publish
        fields = ('name', 'address', 'books')

class AuthorModelSerializer(serializers.ModelSerializer):
    class Meta:
        model=models.Author
        fields=('name','sex','mobile','mobile_in')
        extra_kwargs={
            'mobile':{
                'read_only': True
            },
        }

    mobile_in=serializers.CharField(write_only=True)
    # def validate_mobile_in(self, data):
    #     print(data)
    #     return data

    def create(self, validated_data):
        print(validated_data)
        mobile=validated_data.pop('mobile_in')
        author=models.Author.objects.create(**validated_data)
        authordetail=models.AuthorDetail.objects.create(mobile=mobile,author=author)
        return author
```

## [models.py](http://models.py)****

```
from django.db import models

# 一、基表
# Model类的内部配置Meta类要设置abstract=True，这样的Model类就是用来作为基表

# 多表：Book，Publish，Author，AuthorDetail
class BaseModel(models.Model):
    is_delete = models.BooleanField(default=False)
    create_time = models.DateTimeField(auto_now_add=True)
    class Meta:
        # 基表必须设置abstract，基表就是给普通Model类继承使用的，设置了abstract就不会完成数据库迁移完成建表
        abstract = True

class Book(BaseModel):
    name = models.CharField(max_length=16)
    price = models.DecimalField(max_digits=5, decimal_places=2)
    publish = models.ForeignKey(to='Publish', related_name='books', db_constraint=False, on_delete=models.DO_NOTHING)
    # 重点：多对多外键实际在关系表中，ORM默认关系表中两个外键都是级联
    # ManyToManyField字段不提供设置on_delete，如果想设置关系表级联，只能手动定义关系表
    authors = models.ManyToManyField(to='Author', related_name='books', db_constraint=False)

    # 自定义连表深度，不需要反序列化，因为自定义插拔属性不参与反序列化
    @property
    def publish_name(self):
        return self.publish.name
    @property
    def author_list(self):
        temp_author_list = []
        for author in self.authors.all():
            temp_author_list.append({
                'name': author.name,
                'sex': author.get_sex_display(),
                'mobile': author.detail.mobile
            })
        return temp_author_list

class Publish(BaseModel):
    name = models.CharField(max_length=16)
    address = models.CharField(max_length=64)

class Author(BaseModel):
    name = models.CharField(max_length=16)
    sex = models.IntegerField(choices=[(0, '男'),(1, '女')], default=0)

class AuthorDetail(BaseModel):
    mobile = models.CharField(max_length=11)
    # 有作者可以没有详情，删除作者，详情一定会被级联删除
    # 外键字段为正向查询字段，related_name是反向查询字段
    author = models.OneToOneField(to='Author', related_name='detail', db_constraint=False, on_delete=models.CASCADE)

# 二、表断关联
# 1、表之间没有外键关联，但是有外键逻辑关联(有充当外键的字段)
# 2、断关联后不会影响数据库查询效率，但是会极大提高数据库增删改效率（不影响增删改查操作）
# 3、断关联一定要通过逻辑保证表之间数据的安全
# 4、断关联
# 5、级联关系
#       作者没了，详情也没：on_delete=models.CASCADE
#       出版社没了，书还是那个出版社出版：on_delete=models.DO_NOTHING
#       部门没了，员工没有部门(空不能)：null=True, on_delete=models.SET_NULL
#       部门没了，员工进入默认部门(默认值)：default=0, on_delete=models.SET_DEFAULT

# 三、ORM外键设计
# 1、一对多：外键放在多的一方
# 2、多对多：外键放在常用的一方
# 3、一对一：外键放在不常用的一方
# 4、外键字段为正向查询字段，related_name是反向查询字段

# from django.contrib.auth.models import AbstractUser, User
# class MyUser(AbstractUser):
#     pass
```

## [setting.py](http://setting.py)****

```
LANGUAGE_CODE = 'zh-hans'

TIME_ZONE = 'Asia/shanghai'

USE_I18N = True

USE_L10N = True

USE_TZ = False
```

## [urls.py](http://urls.py)****

```
path(r'publishes/', views.PublishAPIView.as_view()),
re_path(r'^publishes/(?P<pk>\\d+)/$', views.PublishAPIView.as_view()),

path(r'books/', views.BookAPIView.as_view()),
re_path(r'^books/(?P<pk>\\d+)/$', views.BookAPIView.as_view()),
```

[← linux/入门到精通/17-Linux计划任务](http://www.liuqingzheng.top/linux/%E5%85%A5%E9%97%A8%E5%88%B0%E7%B2%BE%E9%80%9A/17-Linux%E8%AE%A1%E5%88%92%E4%BB%BB%E5%8A%A1/)​[python/Django-rest-framework框架/12-RBAC-基于角色的访问控制 →](http://www.liuqingzheng.top/python/Django-rest-framework%E6%A1%86%E6%9E%B6/12-RBAC-%E5%9F%BA%E4%BA%8E%E8%A7%92%E8%89%B2%E7%9A%84%E8%AE%BF%E9%97%AE%E6%8E%A7%E5%88%B6/)

赏