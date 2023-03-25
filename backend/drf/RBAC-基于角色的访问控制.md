---
title: RBAC-基于角色的访问控制
description: 
published: true
date: 2023-03-06T05:40:30.694Z
tags: 
editor: markdown
dateCreated: 2023-02-26T08:43:52.670Z
---

## RBAC-基于角色的访问控制

2020-07-14

阅读数：584次

**RBAC-基于角色的访问控制 一 什么是RBAC 概念**

`1`

`RBAC  是基于角色的访问控制（Role-Based Access Control ）在 RBAC  中，权限与角色相关联，用户通过成为适当角色的成员而得到这些角色的权限。这就极大地简化了权限的管理。这样管理都是层级相互依赖的，权限赋予给角色，而把角色又赋予用户，这样的权限设计很清楚，管理起来很方便。`​**应用**

`123456`

`# RBAC - Role-Based Access Control# Django的 Auth组件 采用的认证规则就是RBAC# 1）像专门做人员权限管理的系统（CRM系统）都是公司内部使用，所以数据量都在10w一下，一般效率要求也不是很高# 2）用户量极大的常规项目，会分两种用户：前台用户(三大认证) 和 后台用户(BRAC来管理)# 结论：没有特殊要求的Django项目可以直接采用Auth组件的权限六表，不需要自定义六个表，也不需要断开表关系，单可能需要自定义User表`​**前后台权限控制**

`123456`

`# 1）后台用户对各表操作，是后台项目完成的，我们可以直接借助admin后台项目（Django自带的）# 2）后期也可以用xadmin框架来做后台用户权限管理# 3）前台用户的权限管理如何处理#   定义了一堆数据接口的视图类，不同的登录用户是否能访问这些视图类，能就代表有权限，不能就代表无权限#   前台用户权限用drf框架的 三大认证`​**二 Django的内置RBAC(六表) 权限三表 权限六表 三 实操 ​**​[models.py](http://models.py)****

`12345678910111213141516171819202122`

`from django.db import modelsfrom django.contrib.auth.models import AbstractUserclass User(AbstractUser):    mobile = models.CharField(max_length=11, unique=True)    def __str__(self):        return self.usernameclass Book(models.Model):    name = models.CharField(max_length=64)    def __str__(self):        return self.nameclass Car(models.Model):    name = models.CharField(max_length=64)    def __str__(self):        return self.name`​[admin.py](http://admin.py)****

`1234567891011121314151617181920`

`from . import modelsfrom django.contrib.auth.admin import UserAdmin as DjangoUserAdmin# 自定义User表后，admin界面管理User类class UserAdmin(DjangoUserAdmin):    # 添加用户课操作字段    add_fieldsets = (        (None, {            'classes': ('wide',),            'fields': ('username', 'password1', 'password2', 'is_staff', 'mobile', 'groups', 'user_permissions'),        }),    )    # 展示用户呈现的字段    list_display = ('username', 'mobile', 'is_staff', 'is_active', 'is_superuser')admin.site.register(models.User, UserAdmin)admin.site.register(models.Book)admin.site.register(models.Car)` 这样就可以登陆到admin后台进行操作了

![https://tva1.sinaimg.cn/large/007S8ZIlgy1ggqmxia622j317j08gmy2.jpg](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggqmxia622j317j08gmy2.jpg)

![https://tva1.sinaimg.cn/large/007S8ZIlgy1ggqmxvp38zj319t0jlgnb.jpg](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggqmxvp38zj319t0jlgnb.jpg)