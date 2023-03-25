---
title: 常用操作
description: 
published: true
date: 2023-03-06T03:04:01.430Z
tags: 
editor: markdown
dateCreated: 2023-02-25T10:59:03.132Z
---

## 对齐

1. 居中对齐文本

    如果仅需在元素内居中文本，请使用 `text-align: center;`：
2. 居中对齐图像

    ```css
    img {
      display: block;
      margin-left: auto;
      margin-right: auto;
      width: 40%;
    }
    ```
3. 左和右对齐 - 使用 position

    ```css
    .right {
      position: absolute;
      right: 0px;
      width: 300px;
      border: 3px solid #73AD21;
      padding: 20px;
    }
    ```

    ```css
    .right {
      float: right;
      width: 300px;
      border: 3px solid #73AD21;
      padding: 10px;
    }
    ```
4. 垂直对齐 - 使用 padding

    ```css
    .center {
      padding: 70px 0;
      border: 3px solid green;
      text-align: center;
    }
    ```

    ```css
    .center {
      line-height: 200px;
      height: 200px;
      border: 3px solid green;
      text-align: center;
    }

    /* 如果有多行文本，请添加如下代码：*/
    .center p {
      line-height: 1.5;
      display: inline-block;
      vertical-align: middle;
    }
    ```

    ```css
    .center { 
      height: 200px;
      position: relative;
      border: 3px solid green; 
    }

    .center p {
      margin: 0;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
    }
    ```

    ```css
    .center {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 200px;
      border: 3px solid green; 
    }
    ```
    `@supports`

@supports我们经常在css中遇到，@supports是用来检测浏览器是否支持css的某个属性。通常我们可以用它来处理浏览器的兼容性的问题。用@supports来判断浏览器是否支持这个css属性，如果支持的话，我们写的css样式就会起作用，否则的话不会起作用,例如下面的例子，如果浏览器支持display的话，则会执行下面的样式。同时它的逻辑操作符有and,or,not。

![assets/image-20221030150950-8ysaskh.png](assets/image-20221030150950-8ysaskh.png)

**去除页面滚动条**

```
/*IE*/
-ms-overflow-style:none;
/*火狐*/
overflow:-moz-scrollbars-none;

//谷歌
::-webkit-scrollbar{
    display:none;
}
```

### 毛玻璃效果

```bash
backdrop-filter: blur(1px)
```

### 文字淡入淡出

index.html

```bash
<div class="box_test">
        <div class="circle_t"></div>
        CSS文字淡入淡出,CSS文字淡入淡出
    </div>
```

index.css

```bash
body {
    background-color: black;
    font-size: 30px;
}

.box_test {
    width: 500px;
    height: 40px;
    color: white;
    line-height: 40px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    -webkit-transform: translate(-50%, -50%);
    -moz-transform: translate(-50%, -50%);
    -ms-transform: translate(-50%, -50%);
    -o-transform: translate(-50%, -50%);
    overflow: hidden;
}

@keyframes move-cover{
    0%{
        left: 0;
    }
    50%{
        left: 100%;
    }
    100%{
        left: 0;
    }
}
.circle_t {
    width: 500px;
    height: 100px;
    background-color: black;
    position: absolute;
    left: 0;
    top: -40px;
    box-shadow: 0 0 100px 10px white;
    animation: move-cover 4s ease-in-out infinite;
    -webkit-animation: move-cover 4s ease-in-out infinite;
}
```

## 模糊

```
‍‍```python
filter: blur(200px);
‍‍```
```