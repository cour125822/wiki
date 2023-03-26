---
title: Serializer
description: 
published: true
date: 2023-03-26T08:04:30.466Z
tags: 
editor: markdown
dateCreated: 2023-02-26T08:35:20.687Z
---

# **Serializer**

作用：

```
1. 序列化,序列化器会把模型对象转换成字典,经过response以后变成json字符串
2. 反序列化,把客户端发送过来的数据,经过request以后变成字典,序列化器可以把字典转成模型
3. 反序列化,完成数据校验功能
```

## **1.1 定义序列化器**

> Django REST framework 中的Serializer 使用类来定义，须继承自rest_framework.serializers.Serializer。

为了方便视频序列化器的使用，我们先创建一个新的子应用服务器

```
python manage.py startapp sers
```

我们已经有了一个数据库模型类students/Student

```
from django.db import models

# Create your models here.
class Student(models.Model):
    # 模型字段
    name = models.CharField(max_length=100,verbose_name="姓名",help_text="提示文本:账号不能为空！")
    sex = models.BooleanField(default=True,verbose_name="性别")
    age = models.IntegerField(verbose_name="年龄")
    class_null = models.CharField(max_length=5,verbose_name="班级编号")
    description = models.TextField(verbose_name="个性签名")

    class Meta:
        db_table="tb_student"
        verbose_name = "学生"
        verbose_name_plural = verbose_name
```

我们想为这个模型类提供一个序列化器，可以定义如下：

```
from rest_framework import serializers

# 声明序列化器，所有的序列化器都要直接或者间接继承于 Serializer
# 其中，ModelSerializer是Serializer的子类，ModelSerializer在Serializer的基础上进行了代码简化
class StudentSerializer(serializers.Serializer):
    """学生信息序列化器"""
    # 1. 需要进行数据转换的字段
    id = serializers.IntegerField()
    name = serializers.CharField()
    age = serializers.IntegerField()
    sex = serializers.BooleanField()
    description = serializers.CharField()

    # 2. 如果序列化器集成的是ModelSerializer，则需要声明调用的模型信息

    # 3. 验证代码

    # 4. 编写添加和更新模型的代码
```

**注意：serializer不是只能为数据库模型类定义的，也可以为非数据库模型类的数据定义。** serializer是独立于数据库之外的存在的。

**常用字段类型**：

| 场地           | 构造方式                                                                                                                                                                                                                                                       |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 布尔字段       | 布尔字段（）                                                                                                                                                                                                                                                   |
| Null布尔字段   | NullBooleanField()                                                                                                                                                                                                                                             |
| 字符域         | CharField(max_length=None, min_length=None, allow_blank=False, trim_whitespace=True)                                                                                                                                                                           |
| 电子邮件字段   | 电子邮件字段（最大长度=无，最小长度=无，允许空白=假）                                                                                                                                                                                                          |
| 正则表达式字段 | 正则表达式（正则表达式，max_length=None，min_length=None，allow_blank=False）                                                                                                                                                                                  |
| 蛞蝓场         | SlugField(max length=50, min_length=None, allow_blank=False) 正则字段，验证正则模式 [a-zA-Z0-9 -]+                                                                                                                                                             |
| 网址字段       | URLField(max_length=200, min_length=None, allow_blank=False)                                                                                                                                                                                                   |
| UUID 字段      | UUIDField(format=’hex_verbose’) 格式：1）'hex_verbose'如"5ce0e9a5-5ffa-654b-cee0-1238041fb31a" 2）'hex'如"5ce0e9a55ffa654bcee01238041fb31a" 3）'int'-："123456789012312313134124512351145145114" 4）'urn'如："urn:uuid:5ce0e9a5-5ffa-654b-cee0-1238041fb31a" |
| IP地址字段     | IPAddressField(protocol=’both’, unpack_ipv4=False, **options)                                                                                                                                                                                                |
| 整数字段       | 整数字段（最大值=无，最小值=无）                                                                                                                                                                                                                               |
| 浮点场         | FloatField(max_value=None, min_value=None)                                                                                                                                                                                                                     |
| 十进制字段     | DecimalField(max_digits, decimal_places,coerce_to_string=None, max_value=None, min_value=None) max_digits: 最多曝光 decimal_palces: 小数位                                                                                                                     |
| 日期时间字段   | DateTimeField(格式=api_settings.DATETIME_FORMAT, input_formats=None)                                                                                                                                                                                           |
| 日期字段       | 日期字段（格式=api_settings.DATE_FORMAT，输入格式=无）                                                                                                                                                                                                         |
| 时间场         | 时间字段（格式=api_settings.TIME_FORMAT，输入格式=无）                                                                                                                                                                                                         |
| 持续时间字段   | 持续时间字段（）                                                                                                                                                                                                                                               |
| 选择字段       | ChoiceField(choices)choices与Django的用法相同                                                                                                                                                                                                                  |
| 多选字段       | MultipleChoiceField（选择）                                                                                                                                                                                                                                    |
| 文件字段       | FileField(max_length=None, allow_empty_file=False, use_url=UPLOADED_FILES_USE_URL)                                                                                                                                                                             |
| 图像域         | ImageField(max_length=None, allow_empty_file=False, use_url=UPLOADED_FILES_USE_URL)                                                                                                                                                                            |
| 列表字段       | ListField(child=, min_length=None, max_length=None)                                                                                                                                                                                                            |
| 字典域         | 字典字段（孩子=）                                                                                                                                                                                                                                              |

**参数：**

| 参数名称 | 作用             |
| ---------- | ------------------ |
| 最长长度 | 最大长度         |
| 最小长度 | 最小长度         |
| 允许空白 | 是否允许为空     |
| 修剪空白 | 是否有断空白字符 |
| 最大值   | 数               |
| 最小值   | 问号             |

通用参数：

| 参数名称 | 说明                                        |
| ---------- | --------------------------------------------- |
| 只读     | 默认输出字段仅用于序列化，默认为False       |
| 只写     | 最初该字段仅用于反序列化输入，默认为False   |
| 必需的   | 最初该字段在反序列化时必须输入，默认为 True |
| 默认     | 反序列化时使用的默认值                      |
| 允许为空 | 看似该确实是否允许没有，默认错误            |
| 验证者   | 该场地使用的验证器                          |
| 错误消息 | 包含错误编号与错误信息的字典                |
| 标签     | 用于展示页面时，显示的 HTML API 名称        |
| 帮助文本 | HTML 展示页面时，显示字段帮助 API 提示信息  |

## **1.2 创建Serializer对象**

定义好Serializer类之后，就可以创建Serializer对象了。

序列化器的构造方法为：

```
Serializer(instance=None, data=empty, **kwarg)
```

说明：

1 用于序列时，将模型类对象化**实例**参数

**2）将反序列化的数据用于数据**的反序列化时

3）除了ins参数外，在构造时通过额外的**数据**，如

```
serializer = AccountSerializer(account, context={'request': request})
```

**通过上下文参数附加的数据，可以通过Serializer对象的上下文属性获取。**

1. 使用顺序化器的时候一定要注意，以后顺序化器声明了，不会自动执行，需要我们在视图中进行调用才可以。
2. 化器无法直接接收数据，需要我们在视图中创建序列化器对象时把使用的数据传递过来。
3. 序列化器字段类似于我们前面使用过的表单系统。
4. 开发restful api时，序列化器会帮我们把模型数据转换成字典。
5. drf提供的视图会帮我们把字典转换成json，或者把客户端发送过来的数据转换字典。

## **1.3 序列化器的使用**

序列化器的使用分两个阶段：

1. 在客户端请求的时候，使用序列化器可以完成对数据的反序列化。
2. 在服务器响应的时候，使用序列化器可以完成对数据的序列化。

### **1.3.1 顺序化**

### **1.3.1.1 基本使用**

1）先查询出一个学生对象

```
from students.models import Student

student = Student.objects.get(id=3)
```

2） 构造序列化器对象

```
from .serializers import StudentSerializer

serializer = StudentSerializer(instance=student)
```

3）获取序列化数据

通过数据属性可以获取序列化后的数据

```
serializer.data
# {'id': 4, 'name': '小张', 'age': 18, 'sex': True, 'description': '猴赛雷'}
```

完整视图代码：

```
from django.views import View
from students.models import Student
from .serializers import StudentSerializer
from django.http.response import JsonResponse
class StudentView(View):
    """使用序列化器序列化转换单个模型数据"""
    def get(self,request,pk):
        # 获取数据
        student = Student.objects.get(pk=pk)
        # 数据转换[序列化过程]
        serializer = StudentSerializer(instance=student)
        print(serializer.data)
        # 响应数据
        return JsonResponse(serializer.data)
```

4）如果要被序列化是包含多条数据的查询集QuerySet，可以通过添加**many=True**参数补充说明

```
"""使用序列化器序列化转换多个模型数据"""
def get(self,request):
    # 获取数据
    student_list = Student.objects.all()

    # 转换数据[序列化过程]
    # 如果转换多个模型对象数据，则需要加上many=True
    serializer = StudentSerializer(instance=student_list,many=True)
    print( serializer.data ) # 序列化器转换后的数据

    # 响应数据给客户端
    # 返回的json数据，如果是列表，则需要声明safe=False
    return JsonResponse(serializer.data,safe=False)

# 访问结果：
# [OrderedDict([('id', 1), ('name', 'xiaoming'), ('age', 20), ('sex', True), ('description', '测试')]), OrderedDict([('id', 2), ('name', 'xiaohui'), ('age', 22), ('sex', True), ('description', '后面来的测试')]), OrderedDict([('id', 4), ('name', '小张'), ('age', 18), ('sex', True), ('description', '猴赛雷')])]
```

### **1.3.1.2 高级用法**

```
#source和serializers.SerializerMethodField()的用法
# models.py
from django.db import models

class Book(models.Model):
    title=models.CharField(max_length=32)
    price=models.IntegerField()
    pub_date=models.DateField()
    publish=models.ForeignKey("Publish",on_delete=models.CASCADE,null=True)
    authors=models.ManyToManyField("Author")
    def __str__(self):
        return self.title

class Publish(models.Model):
    name=models.CharField(max_length=32)
    email=models.EmailField()
    def __str__(self):
        return self.name

class Author(models.Model):
    name=models.CharField(max_length=32)
    age=models.IntegerField()
    def __str__(self):
        return self.name
# ser.py
from rest_framework import serializers
from app01.models import Book
class BookSerializers(serializers.Serializer):
    id=serializers.CharField(read_only=True)
    title=serializers.CharField(max_length=32)
    price=serializers.IntegerField()
    pub_date=serializers.DateField()
    # publish=serializers.CharField(source="publish.name",read_only=True)
    publish=serializers.CharField(source="publish.name",default='xxx')
    #authors=serializers.CharField(source="authors.all")
    authors=serializers.SerializerMethodField(read_only=True)
    def get_authors(self,obj):
        temp=[]
        for author in obj.authors.all():
            temp.append(author.name)
        return temp

    def create(self, validated_data):
        print(validated_data)
        publish_id=validated_data.get('publish').get('name')
        print(publish_id)
        del validated_data['publish']
        return Book.objects.create(publish_id=publish_id,**validated_data)

    def update(self, instance, validated_data):
        print(validated_data.get('aa'))
        instance.title = validated_data.get('title', instance.title)
        instance.price = validated_data.get('price', instance.price)
        instance.pub_date = validated_data.get('pub_date', instance.pub_date)
        print(validated_data.get('publish', instance.publish))
        instance.publish_id = validated_data.get('publish', instance.publish).get('name')
        instance.save()
        return instance
# views.py
from django.shortcuts import render,HttpResponse
from app01 import models

from django.http import HttpRequest

from rest_framework.views import APIView
from app01.models import Book
from rest_framework.response import Response
from app01.ser import BookSerializers
class BookViewSet(APIView):

    def get(self,request,*args,**kwargs):
        book_list=Book.objects.all()
        # 序列化方式3:
        bs=BookSerializers(book_list,many=True)     #many=True代表有多条数据，如果只有一条数据，many=False
        return Response(bs.data)
    def post(self,request,*args,**kwargs):

        bs=BookSerializers(data=request.data)
        bs.is_valid(raise_exception=True)
        # print(bs.validated_data)
        bs.save()
        return Response(bs.data)

class BookDetailView(APIView):
    def get(self,request,pk):
        book_obj=models.Book.objects.filter(pk=pk).first()
        bs=BookSerializers(book_obj,many=False)
        return Response(bs.data)
    def put(self,request,pk):
        book_obj = models.Book.objects.filter(pk=pk).first()

        bs=BookSerializers(instance=book_obj,data=request.data,partial=True)
        if bs.is_valid():
            bs.save(aa="lqz") # update
            return Response(bs.data)
        else:
            return Response(bs.errors)
    def delete(self,request,pk):
        models.Book.objects.filter(pk=pk).delete()

        return Response("")
# urls.py
from django.contrib import admin
from django.urls import path,re_path
from app01 import views
urlpatterns = [
    path('admin/', admin.site.urls),
    path('books/', views.BookViewSet.as_view()),
    re_path('books/(?P<pk>\\d+)/', views.BookDetailView.as_view()),
]
```

注意：

source 如果是字段，会显示字段，如果是方法，会执行方法，使用加（authors=serializers.CharField(source=’authors.all’)）

如在模型中定义一个方法，直接可以在源中指定执行

```
class UserInfo(models.Model):
    user_type_choices = (
        (1,'普通用户'),
        (2,'VIP'),
        (3,'SVIP'),
    )
    user_type = models.IntegerField(choices=user_type_choices)

    username = models.CharField(max_length=32,unique=True)
    password = models.CharField(max_length=64)

#视图
ret=models.UserInfo.objects.filter(pk=1).first()
aa=ret.get_user_type_display()

#serializer
xx=serializers.CharField(source='get_user_type_display')
```

### **1.3.2 反序列化**

### **1.3.2.1 数据验证**

使用序列化器能够进行反序列化时，需要对数据进行验证后，获取验证成功的数据或保存成模型类对象。

在获取反序列化的数据前，必须调用**is_valid()**方法进行验证，验证成功返回True，否则返回False。

验证时，可以通过序列化器对象的**错误**修改属性获取错误信息返回查询，包含了字段和字段的**错误**。

验证成功，可以通过序列化器对象的**validated_data**属性获取数据。

在定义顺序器的时候，指定每个选项的参数化的顺序化类型和类型，本身就是一种验证。

比如我们前面定义过的BookInfoSerializer

```
class BookInfoSerializer(serializers.Serializer):
    """图书数据序列化器"""
    id = serializers.IntegerField(label='ID', read_only=True)
    btitle = serializers.CharField(label='名称', max_length=20)
    bpub_date = serializers.DateField(label='发布日期', required=False)
    bread = serializers.IntegerField(label='阅读量', required=False)
    bcomment = serializers.IntegerField(label='评论量', required=False)
    image = serializers.ImageField(label='图片', required=False)
```

通过反化器对象，要将数据序列化序列化给数据构造参数，进行构造验证

```
from booktest.serializers import BookInfoSerializer
data = {'bpub_date': 123}
serializer = BookInfoSerializer(data=data)
serializer.is_valid()  # 返回False
serializer.errors
# {'btitle': [ErrorDetail(string='This field is required.', code='required')], 'bpub_date': [ErrorDetail(string='Date has wrong format. Use one of these formats instead: YYYY[-MM[-DD]].', code='invalid')]}
serializer.validated_data  # {}

data = {'btitle': 'python'}
serializer = BookInfoSerializer(data=data)
serializer.is_valid()  # True
serializer.errors  # {}
serializer.validated_data  #  OrderedDict([('btitle', 'python')])
```

is_valid()方法还可以在验证失败时抛出异常serializers.ValidationError，可以通过传递**trueraise_exception=**参数开启，REST framework接收到这个异常，会向前端返回HTTP 400 Bad Request响应。

```
# Return a 400 response if the data was invalid.
serializer.is_valid(raise_exception=True)
```

觉得这些还需要重新定义验证行为，可以使用以下方法：

### **1) validate_字段名**

对`<field_name>`场地进行验证，如

```
class BookInfoSerializer(serializers.Serializer):
    """图书数据序列化器"""
    ...

    def validate_btitle(self, value):
        if 'django' not in value.lower():
            raise serializers.ValidationError("图书不是关于Django的")
        return value
```

测试

```
from booktest.serializers import BookInfoSerializer
data = {'btitle': 'python'}
serializer = BookInfoSerializer(data=data)
serializer.is_valid()  # False
serializer.errors
#  {'btitle': [ErrorDetail(string='图书不是关于Django的', code='invalid')]}
```

### **2) 验证**

在序列器中需要同时对多个字段进行比较验证的时间，可以定义来验证，如

```
class BookInfoSerializer(serializers.Serializer):
    """图书数据序列化器"""
    ...

    def validate(self, attrs):
        bread = attrs['bread']
        bcomment = attrs['bcomment']
        if bread < bcomment:
            raise serializers.ValidationError('阅读量小于评论量')
        return attrs
```

测试

```
from booktest.serializers import BookInfoSerializer
data = {'btitle': 'about django', 'bread': 10, 'bcomment': 20}
s = BookInfoSerializer(data=data)
s.is_valid()  # False
s.errors
#  {'non_field_errors': [ErrorDetail(string='阅读量小于评论量', code='invalid')]}
```

### **3) 验证者**

在字段中添加验证器选项参数，也可以添加验证行为，如

```
def about_django(value):
    if 'django' not in value.lower():
        raise serializers.ValidationError("图书不是关于Django的")

class BookInfoSerializer(serializers.Serializer):
    """图书数据序列化器"""
    id = serializers.IntegerField(label='ID', read_only=True)
    btitle = serializers.CharField(label='名称', max_length=20, validators=[about_django])
    bpub_date = serializers.DateField(label='发布日期', required=False)
    bread = serializers.IntegerField(label='阅读量', required=False)
    bcomment = serializers.IntegerField(label='评论量', required=False)
    image = serializers.ImageField(label='图片', required=False)
```

测试：

```
from booktest.serializers import BookInfoSerializer
data = {'btitle': 'python'}
serializer = BookInfoSerializer(data=data)
serializer.is_valid()  # False
serializer.errors
#  {'btitle': [ErrorDetail(string='图书不是关于Django的', code='invalid')]}
```

### **1.3.2.2 反序列化-保存数据**

前面的验证数据成功后，我们可以使用序列化器来完成数据反序列化的过程。这个过程可以把数据转成模型类对象。

可以通过实现create()和update()两种方法来实现。

```
class BookInfoSerializer(serializers.Serializer):
    """图书数据序列化器"""
    ...

    def create(self, validated_data):
        """新建"""
        return BookInfo(**validated_data)

    def update(self, instance, validated_data):
        """更新，instance为要更新的对象实例"""
        instance.btitle = validated_data.get('btitle', instance.btitle)
        instance.bpub_date = validated_data.get('bpub_date', instance.bpub_date)
        instance.bread = validated_data.get('bread', instance.bread)
        instance.bcomment = validated_data.get('bcomment', instance.bcomment)
        return instance
```

如果需要在返回数据对象的时候，也将数据保存到数据库中进行修改，则可以改为

```
class BookInfoSerializer(serializers.Serializer):
    """图书数据序列化器"""
    ...

    def create(self, validated_data):
        """新建"""
        return BookInfo.objects.create(**validated_data)

    def update(self, instance, validated_data):
        """更新，instance为要更新的对象实例"""
        instance.btitle = validated_data.get('btitle', instance.btitle)
        instance.bpub_date = validated_data.get('bpub_date', instance.bpub_date)
        instance.bread = validated_data.get('bread', instance.bread)
        instance.bcomment = validated_data.get('bcomment', instance.bcomment)
        instance.save()
        return instance
```

实现了上述两个方法后，在顺序化数据的时候，就可以通过save()方法返回一个数据对象实例了

```
book = serializer.save()
```

如果创建序列化器对象的时候，没有传递实例实例，则调用save()方法的时候，create()被调用，相反，如果传递了实例实例，则调用save()方法的时候，update()调用。

```
from db.serializers import BookInfoSerializer
data = {'btitle': '封神演义'}
serializer = BookInfoSerializer(data=data)
serializer.is_valid()  # True
serializer.save()  # <BookInfo: 封神演义>

from db.models import BookInfo
book = BookInfo.objects.get(id=2)
data = {'btitle': '倚天剑'}
serializer = BookInfoSerializer(book, data=data)
serializer.is_valid()  # True
serializer.save()  # <BookInfo: 倚天剑>
book.btitle  # '倚天剑'
```

### **1.3.2.3 附加说明**

1）在对序列化器进行save()保存的时候，可以额外传递数据，这些数据可以在create()和update()中的validated_data参数获取到

```
# request.user 是django中记录当前登录用户的模型对象
serializer.save(owner=request.user)
```

2）默认序列化器必须传递所有需要的字段，否则会抛出验证异常。但是我们可以使用部分参数来允许部分字段更新

```
# Update `comment` with partial data
serializer = CommentSerializer(comment, data={'content': u'foo bar'}, partial=True)
```

### **1.3.3 模型类序列化器**

如果想要使用序列化器是我们Django的模型类，DRF为我们提供了ModelSerializer模型类序列化器来帮助我们快速创建一个序列化器类。

ModelSerializer 与经常的Serializer 相同，但提供了：

* 基于模型类自动生成领域
* 基于类自动为Serializer模型生成验证器，比如unique_together
* 包含默认的create()和update()的实现

### **1.3.3.1 定义**

中文我们创建一个BookInfoSerializer

```
class BookInfoSerializer(serializers.ModelSerializer):
    """图书数据序列化器"""
    class Meta:
        model = BookInfo
        fields = '__all__'
```

* model 指明参照哪个模型类
* 字段指明为模型类的哪些字段生成

我们可以在python [manage.py](http://manage.py) shell中查看自动生成BookInfoSerializer的具体实现

```
>>> from booktest.serializers import BookInfoSerializer
>>> serializer = BookInfoSerializer()
>>> serializer
BookInfoSerializer():
    id = IntegerField(label='ID', read_only=True)
    btitle = CharField(label='名称', max_length=20)
    bpub_date = DateField(allow_null=True, label='发布日期', required=False)
    bread = IntegerField(label='阅读量', max_value=2147483647, min_value=-2147483648, required=False)
    bcomment = IntegerField(label='评论量', max_value=2147483647, min_value=-2147483648, required=False)
    image = ImageField(allow_null=True, label='图片', max_length=100, required=False)
```

### **1.3.3.2 指定字段**

\1) 使用**字段**来具体字段，`__all__`表名包含所有字段，也可以写明哪些字段，如

```
class BookInfoSerializer(serializers.ModelSerializer):
    """图书数据序列化器"""
    class Meta:
        model = BookInfo
        fields = ('id', 'btitle', 'bpub_date')
```

\2) 使用**排除**可以解决哪些领域

```
class BookInfoSerializer(serializers.ModelSerializer):
    """图书数据序列化器"""
    class Meta:
        model = BookInfo
        exclude = ('image',)
```

\3) 显示字段，例如：

```
class HeroInfoSerializer(serializers.ModelSerializer):
    hbook = BookInfoSerializer()

    class Meta:
        model = HeroInfo
        fields = ('id', 'hname', 'hgender', 'hcomment', 'hbook')
```

\4) 光明前景

**可以通过只读**字段读取_only_fields ，即仅用于顺序化输出的字段

```
class BookInfoSerializer(serializers.ModelSerializer):
    """图书数据序列化器"""
    class Meta:
        model = BookInfo
        fields = ('id', 'btitle', 'bpub_date'， 'bread', 'bcomment')
        read_only_fields = ('id', 'bread', 'bcomment')
```

### **1.3.3.3 添加额外参数选项**

我们可以使用**extra_kwargs**参数为Modelerializer 添加或原有的选项参数

```
class BookInfoSerializer(serializers.ModelSerializer):
    """图书数据序列化器"""
    class Meta:
        model = BookInfo
        fields = ('id', 'btitle', 'bpub_date', 'bread', 'bcomment')
        extra_kwargs = {
            'bread': {'min_value': 0, 'required': True},
            'bcomment': {'min_value': 0, 'required': True},
        }

# BookInfoSerializer():
#    id = IntegerField(label='ID', read_only=True)
#    btitle = CharField(label='名称', max_length=20)
#    bpub_date = DateField(allow_null=True, label='发布日期', required=False)
#    bread = IntegerField(label='阅读量', max_value=2147483647, min_value=0, required=True)
#    bcomment = IntegerField(label='评论量', max_value=2147483647, min_value=0, required=True)
```

## **1.4 不同类型错误分析**

```
#is_valid---->self.run_validation-(执行Serializer的run_validation)-->self.to_internal_value(data)---（执行Serializer的run_validation：485行）
```

![https://tva1.sinaimg.cn/large/007S8ZIlgy1gghkhucudgj315w0rmqv5.jpg](https://tva1.sinaimg.cn/large/007S8ZIlgy1gghkhucudgj315w0rmqv5.jpg)

**image-20200706212122698**

![https://tva1.sinaimg.cn/large/007S8ZIlgy1gghkj4qalrj31yg0qo1kz.jpg](https://tva1.sinaimg.cn/large/007S8ZIlgy1gghkj4qalrj31yg0qo1kz.jpg)

**image-20200706212239321**

## **1.5 顺序组件化源分析**

```
序列化组件，先调用__new__方法，如果many=True，生成ListSerializer对象，如果为False，生成Serializer对象
序列化对象.data方法--调用父类data方法---调用对象自己的to_representation（自定义的序列化类无此方法，去父类找）
Aerializer类里有to_representation方法，for循环执行attribute = field.get_attribute(instance)
再去Field类里去找get_attribute方法，self.source_attrs就是被切分的source，然后执行get_attribute方法，source_attrs
当参数传过去，判断是方法就加括号执行，是属性就把值取出来
```

[← python/Django-rest-framework框架/1-drf-drf入门规范](http://www.liuqingzheng.top/python/Django-rest-framework%E6%A1%86%E6%9E%B6/1-drf-drf%E5%85%A5%E9%97%A8%E8%A7%84%E8%8C%83/)