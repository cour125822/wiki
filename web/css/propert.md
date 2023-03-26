---
title: CSS属性
description: 
published: true
date: 2023-03-26T08:06:55.072Z
tags: 
editor: markdown
dateCreated: 2023-02-25T10:56:18.090Z
---

## 文本属性

| 属性 | 描述                     |
| :----- | :------------------------- |
| [color](https://www.runoob.com/cssref/pr-text-color.html)     | 设置文本颜色             |
| [direction](https://www.runoob.com/cssref/pr-text-direction.html)     | 设置文本方向。           |
| [letter-spacing](https://www.runoob.com/cssref/pr-text-letter-spacing.html)     | 设置字符间距             |
| [line-height](https://www.runoob.com/cssref/pr-dim-line-height.html)     | 设置行高                 |
| [text-align](https://www.runoob.com/cssref/pr-text-text-align.html)     | 对齐元素中的文本         |
| [text-decoration](https://www.runoob.com/cssref/pr-text-text-decoration.html)     | 向文本添加修饰           |
| [text-indent](https://www.runoob.com/cssref/pr-text-text-indent.html)     | 缩进元素中文本的首行     |
| [text-shadow](https://www.runoob.com/cssref/css3-pr-text-shadow.html)     | 设置文本阴影             |
| [text-transform](https://www.runoob.com/cssref/pr-text-text-transform.html)     | 控制元素中的字母         |
| [unicode-bidi](https://www.runoob.com/cssref/pr-text-unicode-bidi.html)     | 设置或返回文本是否被重写 |
| [vertical-align](https://www.runoob.com/cssref/pr-pos-vertical-align.html)     | 设置元素的垂直对齐       |
| [white-space](https://www.runoob.com/cssref/pr-text-white-space.html)     | 设置元素中空白的处理方式 |
| [word-spacing](https://www.runoob.com/cssref/pr-text-word-spacing.html)     | 设置字间距               |

## 字体属性

| Property | 描述                                 |
| :--------- | :------------------------------------- |
| [font](https://www.runoob.com/cssref/pr-font-font.html)         | 在一个声明中设置所有的字体属性       |
| [font-family](https://www.runoob.com/cssref/pr-font-font-family.html)         | 指定文本的字体系列                   |
| [font-size](https://www.runoob.com/cssref/pr-font-font-size.html)         | 指定文本的字体大小                   |
| [font-style](https://www.runoob.com/cssref/pr-font-font-style.html)         | 指定文本的字体样式                   |
| [font-variant](https://www.runoob.com/cssref/pr-font-font-variant.html)         | 以小型大写字体或者正常字体显示文本。 |
| [font-weight](https://www.runoob.com/cssref/pr-font-weight.html)         | 指定字体的粗细。                     |

## 链接

* a:link - 正常，未访问过的链接
* a:visited - 用户已访问过的链接
* a:hover - 当用户鼠标放在链接上时
* a:active - 链接被点击的那一刻

## 列表属性

| 属性 | 描述                                               |
| :----- | :--------------------------------------------------- |
| [list-style](https://www.runoob.com/cssref/pr-list-style.html)     | 简写属性。用于把所有用于列表的属性设置于一个声明中 |
| [list-style-image](https://www.runoob.com/cssref/pr-list-style-image.html)     | 将图像设置为列表项标志。                           |
| [list-style-position](https://www.runoob.com/cssref/pr-list-style-position.html)     | 设置列表中列表项标志的位置。                       |
| [list-style-type](https://www.runoob.com/cssref/pr-list-style-type.html)     | 设置列表项标志的类型。                             |


## 背景

| 属性                  | 描述     |
| ----------------------- | ---------- |
| background-color      | 背景颜色 |
| background-image      | 背景图片 |
| background-repeat     | 背景重复 |
| background-attachment | 背景附着 |
| background-position   | 背景位置 |

## 边框

| 属性 | 描述     |
| ------ | ---------- |
| `border-style`     | 边框类型 |
| `border-width`     | 边框宽度 |
| `border-color`     | 边框颜色 |
| `border-radius`     | 边框圆角 |
| `border`     | `border-width\border-style`\\`border-color`    |

### 边框类型

* `dotted` - 定义点线边框
* `dashed` - 定义虚线边框
* `solid` - 定义实线边框
* `double` - 定义双边框
* `groove` - 定义 3D 坡口边框。效果取决于 border-color 值
* `ridge` - 定义 3D 脊线边框。效果取决于 border-color 值
* `inset` - 定义 3D inset 边框。效果取决于 border-color 值
* `outset` - 定义 3D outset 边框。效果取决于 border-color 值
* `none` - 定义无边框
* `hidden` - 定义隐藏边框

### 边框分边

```css
 border-top-style: dotted;
  border-right-style: solid;
  border-bottom-style: dotted;
  border-left-style: solid;
```



## 渐变

### 线性渐变

必须定义至少两个色标。色标是您要呈现平滑过渡的颜色。您还可以设置起点和方向（或角度）以及渐变效果。

```css
background-image: linear-gradient(direction, color-stop1, color-stop2, ...);
```

### 径向渐变

由其中心定义。

```css
background-image: radial-gradient(shape size at position, start-color, ..., last-color);
```

*size* 参数定义渐变的大小。它可接受四个值：

* *closest-side*
* *farthest-side*
* *closest-corner*
* *farthest-corner*

重复的径向渐变：

```
#grad {
  background-image: repeating-radial-gradient(red, yellow 10%, green 15%);
}
```

| [background-image](https://www.w3school.com.cn/cssref/pr_background-image.asp "CSS background-image 属性") | 为一个元素设置一幅或多幅背景图像。 |
| -- | ------------------------------------ |

## 阴影

### CSS 文字阴影

CSS `text-shadow` 属性为文本添加阴影。

```css
text-shadow: h-shadow v-shadow blur color;
```

| 值 | 描述                             |
| ---- | ---------------------------------- |
| *h-shadow*   | 必需。水平阴影的位置。允许负值。 |
| *v-shadow*   | 必需。垂直阴影的位置。允许负值。 |
| *blur*   | 可选。模糊的距离。               |
| *color*   | 可选。阴影的颜色。参阅 [CSS 颜色值](https://www.runoob.com/cssref/css-colors-legal.html "CSS 合法颜色值")。        |

最简单的用法是只指定水平阴影（2px）和垂直阴影（2px）：`​  text-shadow: 2px 2px;`

### box-shadow



## 2D转换

通过使用 CSS `transform` 属性，您可以利用以下 2D 转换方法：

* `translate()`移动元素
* `rotate()`旋转元素
* `scaleX()`放大、缩小
* `scaleY()`放大、缩小
* `scale()`放大、缩小
* `skewX()`倾斜
* `skewY()`倾斜
* `skew()`倾斜
* `matrix()matrix(scaleX(),skewY(),skewX(),scaleY(),translateX(),translateY())`组合

| 函数          | 描述                                     |
| --------------- | ------------------------------------------ |
| matrix(*n*,*n*,*n*,*n*,*n*,*n*) | 定义 2D 转换，使用六个值的矩阵。         |
| translate(*x*,*y*)  | 定义 2D 转换，沿着 X 和 Y 轴移动元素。   |
| translateX(*n*)  | 定义 2D 转换，沿着 X 轴移动元素。        |
| translateY(*n*)  | 定义 2D 转换，沿着 Y 轴移动元素。        |
| scale(*x*,*y*)      | 定义 2D 缩放转换，改变元素的宽度和高度。 |
| scaleX(*n*)      | 定义 2D 缩放转换，改变元素的宽度。       |
| scaleY(*n*)      | 定义 2D 缩放转换，改变元素的高度。       |
| rotate(*angle*)      | 定义 2D 旋转，在参数中规定角度。         |
| skew(*x-angle*,*y-angle*)       | 定义 2D 倾斜转换，沿着 X 和 Y 轴。       |
| skewX(*angle*)       | 定义 2D 倾斜转换，沿着 X 轴。            |
| skewY(*angle*)       | 定义 2D 倾斜转换，沿着 Y 轴。            |



## 3D转换

通过 CSS `transform` 属性，您可以使用以下 3D 转换方法：

* `rotateX()`
* `rotateY()`
* `rotateZ()`

| 属性 | 描述                                 |
| ------ | -------------------------------------- |
| [transform](https://www.w3school.com.cn/cssref/pr_transform.asp "CSS3 transform 属性")     | 向元素应用 2D 或 3D 转换。           |
| [transform-origin](https://www.w3school.com.cn/cssref/pr_transform-origin.asp "CSS3 transform-origin 属性")     | 允许你改变被转换元素的位置。         |
| [transform-style](https://www.w3school.com.cn/cssref/pr_transform-style.asp "CSS3 transform-style 属性")     | 规定被嵌套元素如何在 3D 空间中显示。 |
| [perspective](https://www.w3school.com.cn/cssref/pr_perspective.asp "CSS3 perspective 属性")     | 规定 3D 元素的透视效果。             |
| [perspective-origin](https://www.w3school.com.cn/cssref/pr_perspective-origin.asp "CSS3 perspective-origin 属性")     | 规定 3D 元素的底部位置。             |
| [backface-visibility](https://www.w3school.com.cn/cssref/pr_backface-visibility.asp "CSS3 backface-visibility 属性")     | 定义元素在不面对屏幕时是否可见。     |

| 函数                        | 描述                                      |
| ----------------------------- | ------------------------------------------- |
| matrix3d(*n*,*n*,*n*,*n*,*n*,*n*,<br />*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*) | 定义 3D 转换，使用 16 个值的 4x4 矩阵。   |
| translate3d(*x*,*y*,*z*)             | 定义 3D 转化。                            |
| translateX(*x*)                | 定义 3D 转化，仅使用用于 X 轴的值。       |
| translateY(*y*)                | 定义 3D 转化，仅使用用于 Y 轴的值。       |
| translateZ(*z*)                | 定义 3D 转化，仅使用用于 Z 轴的值。       |
| scale3d(*x*,*y*,*z*)                 | 定义 3D 缩放转换。                        |
| scaleX(*x*)                    | 定义 3D 缩放转换，通过给定一个 X 轴的值。 |
| scaleY(*y*)                    | 定义 3D 缩放转换，通过给定一个 Y 轴的值。 |
| scaleZ(*z*)                    | 定义 3D 缩放转换，通过给定一个 Z 轴的值。 |
| rotate3d(*x*,*y*,*z*,*angle*)               | 定义 3D 旋转。                            |
| rotateX(*angle*)                   | 定义沿 X 轴的 3D 旋转。                   |
| rotateY(*angle*)                   | 定义沿 Y 轴的 3D 旋转。                   |
| rotateZ(*angle*)                   | 定义沿 Z 轴的 3D 旋转。                   |
| perspective(*n*)               | 定义 3D 转换元素的透视视图。              |


## 过渡

| 属性 | 描述                                         |
| ------ | ---------------------------------------------- |
| [transition](https://www.w3school.com.cn/cssref/pr_transition.asp "CSS transition 属性")     | 简写属性，用于将四个过渡属性设置为单一属性。 |
| [transition-delay](https://www.w3school.com.cn/cssref/pr_transition-delay.asp "CSS transition-delay 属性")     | 规定过渡效果的延迟（以秒计）。               |
| [transition-duration](https://www.w3school.com.cn/cssref/pr_transition-duration.asp "CSS transition-duration 属性")     | 规定过渡效果要持续多少秒或毫秒。             |
| [transition-property](https://www.w3school.com.cn/cssref/pr_transition-property.asp "CSS transition-property 属性")     | 规定过渡效果所针对的 CSS 属性的名称。        |
| [transition-timing-function](https://www.w3school.com.cn/cssref/pr_transition-timing-function.asp "CSS transition-timing-function 属性")     | 规定过渡效果的速度曲线。                     |

### 指定过渡的速度曲线

`transition-timing-function` 属性规定过渡效果的速度曲线。

transition-timing-function 属性可接受以下值：

* `ease` - 规定过渡效果，先缓慢地开始，然后加速，然后缓慢地结束（默认）
* `linear` - 规定从开始到结束具有相同速度的过渡效果
* `ease-in` -规定缓慢开始的过渡效果
* `ease-out` - 规定缓慢结束的过渡效果
* `ease-in-out` - 规定开始和结束较慢的过渡效果
* `cubic-bezier(n,n,n,n)` - 允许您在三次贝塞尔函数中定义自己的值



## 动画

下表列出了 @keyframes 规则和所有 CSS 动画属性：

| 属性 | 描述                                                           |
| ------ | ---------------------------------------------------------------- |
| [@keyframes](https://www.w3school.com.cn/cssref/pr_keyframes.asp "CSS @keyframes 规则")     | 规定动画模式。                                                 |
| [animation](https://www.w3school.com.cn/cssref/pr_animation.asp "CSS animation 属性")     | 设置所有动画属性的简写属性。                                   |
| [animation-delay](https://www.w3school.com.cn/cssref/pr_animation-delay.asp "CSS animation-delay 属性")     | 规定动画开始的延迟。                                           |
| [animation-direction](https://www.w3school.com.cn/cssref/pr_animation-direction.asp "CSS animation-direction 属性")     | 定动画是向前播放、向后播放还是交替播放。                       |
| [animation-duration](https://www.w3school.com.cn/cssref/pr_animation-duration.asp "CSS animation-duration 属性")     | 规定动画完成一个周期应花费的时间。                             |
| [animation-fill-mode](https://www.w3school.com.cn/cssref/pr_animation-fill-mode.asp "CSS animation-fill-mode 属性")     | 规定元素在不播放动画时的样式（在开始前、结束后，或两者同时）。 |
| [animation-iteration-count](https://www.w3school.com.cn/cssref/pr_animation-iteration-count.asp "CSS animation-iteration-count 属性")     | 规定动画应播放的次数。                                         |
| [animation-name](https://www.w3school.com.cn/cssref/pr_animation-name.asp "CSS animation-name 属性")     | 规定 @keyframes 动画的名称。                                   |
| [animation-play-state](https://www.w3school.com.cn/cssref/pr_animation-play-state.asp "CSS animation-play-state 属性")     | 规定动画是运行还是暂停。                                       |
| [animation-timing-function](https://www.w3school.com.cn/cssref/pr_animation-timing-function.asp "CSS animation-timing-function 属性")     | 规定动画的速度曲线。                                           |

```css
/* 动画代码 */
@keyframes example {
  from {background-color: red;}
  to {background-color: yellow;}
}

/* 向此元素应用动画效果 */
div {
  width: 100px;
  height: 100px;
  background-color: red;
  animation-name: example;
  animation-duration: 4s;
}
```

## object-fit: cover

使用 `object-fit: cover;`，它会剪切图像的侧面，保留长宽比，并填充空间，如下所示：