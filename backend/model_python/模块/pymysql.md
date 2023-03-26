---
title: pymysql
description: 
published: true
date: 2023-03-26T08:08:04.367Z
tags: 
editor: markdown
dateCreated: 2023-02-26T08:19:02.289Z
---

# PyMysql

## 安装

```sql
pip3 install pymysql
```

## 操作

### 导入

```sql
import pymysql
```

### 创建链接

```sql
conn = pymysql.connect(    host = '127.0.0.1',    port = 3306,    user = 'root',    password = '123456',    database = 'day48',    charset = 'utf8'  # 编码千万不要加-)  # 链接数据库
```

```sql
cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)username = input('>>>:')password = input('>>>:')sql = "select * from user where name=%s and password=%s"# 不要手动拼接数据 先用%s占位 之后将需要拼接的数据直接交给execute方法即可print(sql)rows = cursor.execute(sql,(username,password))  # 自动识别sql里面的%s用后面元组里面的数据替换if rows:    print('登录成功')    print(cursor.fetchall())else:    print('用户名密码错误')
```

## 参数详解

### connect

| 参数               | 说明                                                               |
| -------------------- | -------------------------------------------------------------------- |
| dsn                | 数据源名称，给出该参数表示数据库依赖                               |
| host=None          | 数据库连接地址                                                     |
| user=None          | 数据库用户名                                                       |
| password=‘’      | 数据库用户密码                                                     |
| database=None      | 要连接的数据库名称                                                 |
| port=3306          | 端口号，默认为3306                                                 |
| charset=‘’       | 要连接的数据库的字符编码（可以在终端登陆mysql后使用 查看，如下图） |
| connect_timeout=10 | 连接数据库的超时时间，默认为10                                     |
| port=3306          | 端口号，默认为3306                                                 |

### connect返回对象

| col1       | col2                                                    |
| ------------ | --------------------------------------------------------- |
| close()    | 关闭数据库的连接                                        |
| commit()   | 提交事务                                                |
| rollback() | 回滚事务                                                |
| cursor()   | 获取游标对象，操作数据库，如执行DML操作，调用存储过程等 |

### 游标对象

| 方法名                                | 说明                                       |
| --------------------------------------- | -------------------------------------------- |
| callproc(procname,[,parameters])      | 调用存储过程，需要数据库支持               |
| close()                               | 关闭当前游标                               |
| execute(operation,[,parameters])      | 执行数据库操作，sql语句或者数据库命令      |
| executemany(operation, seq_of_params) | 用于批量操作                               |
| fetchone()                            | 获取查询结果集合中的下一条记录             |
| fetchmany(size)                       | 获取指定数量的记录                         |
| fetchall()                            | 获取查询结果集合所有记录                   |
| nextset()                             | 跳至下一个可用的数据集                     |
| arraysize                             | 指定使用fetchmany()获取的行数，默认为1     |
| setinputsizes(size)                   | 设置调用execute*()方法时分配的内存区域大小 |
| setoutputsizes(size)                  | 设置列缓冲区大小，对大数据列尤其有用       |

‍

[数据库连接池](https://www.notion.so/3d9c8465564641a5a9b60349219e127a)