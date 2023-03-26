---
title: opencv
description: 
published: true
date: 2023-03-26T08:07:57.883Z
tags: 
editor: markdown
dateCreated: 2023-02-27T12:45:15.005Z
---

# 介绍
> 
# 需要安装的安装包
```shell
pip install opencv-python
pip install opencv-contrib-python
```

# 基础使用
## 查看当前使用的opencv版本
```python
import cv2 
print(cv.__version__)
```

## 文件读取
```python
# 直接读取彩色图,cv2.IMREAD_COLOR可以省列
img = cv2.imread('001.jpg', cv2.IMREAD_COLOR)
# 读取灰度图
img1 = cv2.imread('001.jpg', cv2.IMREAD_GRAYSCALE)

img
# (B,G,R)模式
```

## 显示图像

```python
# 封装一下
def showimages(name, image):
    # 创建一个窗口
    cv2.imshow(name, image)
    # 窗口等待时间, 0不关闭
    cv2.waitKey(0)
    # 窗口点击操作
    cv2.destroyAllWindows()
```

#### 查看图片的属性
```
img.shape
```
```python
# (1280, 1920, 3)
# 1280*1920, 三通道（h, w, c）
showimages('asqdsad', img)
img1.shape
showimages('asdsad', img1)
img1
```
## 图像保存
```python
cv2.imwrite('002.jpg', img1)
```
## 其他操作
```
# 一个numpy数组保存
type(img)
# 查看图片大小
img.size
# 查看类型
img.dtype
```
## 视频读取
```
# 读取视频
vc = cv2.VideoCapture('test.mp4')
vc.isOpened is True
# 判断视频是否读取成功
if vc.isOpened():
    open, frame = vc.read()
else:
    open = False
while open:
    ret, frame = vc.read()
    if frame is None:
        break
    if ret == True:
        # 转换成灰度图
        gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
        cv2.imshow('result', gray)
        if cv2.waitKey(10) & 0xFF == 27:
            break
vc.release()
cv2.destroyAllWindows()
```
## 截取部分图像
> 利用的是numpy的方法进行截取像素
```
cat = img[0:150,0:200]
showimages('cata', cat)
```
## 提取通道颜色
```
b, g, r = cv2.split(img)
b
g
r
```
## BGR组合图像
```
img2 = cv2.merge((b, g, r))
img2
```
## 图像复制
```
img1 = ing.copy()
```
## 边界填充
```
showimages('sadsa', img)
img.shape
top_size, bottom_size, left_size, right_size = (50,50,50,50)
# borderType:   填充方式
# cv2.BORDER_REPLICATE          复制法        
# cv2.BORDER_REFLECT            反射法
# cv2.BORDER_REFLECT_101        反射法
# cv2.BORDER_WRAP               外包装
# cv2.BORDER_CONSTANT, value=0  常量法

replicate = cv2.copyMakeBorder(img, top_size, bottom_size, left_size, right_size, cv2.BORDER_REPLICATE)
# replicate = cv2.copyMakeBorder(img, top_size, bottom_size, left_size, right_size, cv2.BORDER_REFLECT)
# replicate = cv2.copyMakeBorder(img, top_size, bottom_size, left_size, right_size, cv2.BORDER_REFLECT_101)
# replicate = cv2.copyMakeBorder(img, top_size, bottom_size, left_size, right_size, cv2.BORDER_WRAP)
# replicate = cv2.copyMakeBorder(img, top_size, bottom_size, left_size, right_size, cv2.BORDER_CONSTANT, value=0)

showimages('asdasdsa', replicate)
```
## 数值计算
```
img3 = img + 10 # 超出取余
img4 = add(img1, img2) #越界255
```
## 图像大小改变
```
img4 = cv2.resize(img, (700, 500))
# img4 = cv2.resize(img, (0, 0), fx=3, fy=2)
showimages('resizr', img4)
```
## 图像相加
```
# res = cv2.addWeighted(图像1, 图像一权重, 图像2, 图像二权重, 偏置)
```
## 图像阈值

`ret, dst = cv2.threshold(src, thresh, maxval, type)`
- `src`: 输入图，只能单通道，一般是灰度图
- `dst`：输出图
- `thresh`：阈值
- `maxval`：当像素超过阈值根据type来决定（或者小于域值，根据type决定），所赋予的值
- `type`：二极化操作类型，包含以下五种类型
		1. cv2.THRESH_BINARY       超过阈值部分取最大值
  	2. cv2.THRESH_BINARY_INV   THRESH_BINARY反转
		3. cv2.THRESH_TRUNC        大于阈值部分设为阈值，否则不变
		4. cv2.THRESH_TOZERO       大于阈值部分不改变，否则设为0
		5. cv2.THRESH_TOZERO_INV   THRESH_TOZERO反转
#  图像平滑处理
## 均值滤波
> 简单的平均卷积操作
```
blur = cv2.blur(img, (3, 3))
```
## 方框滤波
> 和基本均值一样，可以选择归一化，容易越界
```
box = cv2.boxFilter(img, -1, (3,3), normalize=True)
```
## 高斯滤波
> 高斯滤波的卷积核里的数值是满足高斯分布的，相当于更加重视中间的值
```
aussion = cv2.GaussianBlur(img, (5,5), 1)
```
## 中值滤波
> 相当于中值替代
```python
median = cv2.medianBlur(img, 5)
```



# 形态学

## 腐蚀操作
> 使用内核，边界处被腐蚀
```python
# 创建核心
kernel = np.ones((5,5), np.uint8)
# 腐蚀操作, iterations腐蚀次数
erosion_1 = cv2.erode(img, kernel, iterations=1)
```


## 膨胀操作
> 与腐蚀操作相反
```python
# 创建核心
kernel = np.ones((5,5), np.uint8)
dilate_1 = cv2.dilate(img, kernel, iterations=1)
```

> 操作时，先腐蚀去除无用，在使用膨胀恢复有需要的

## 开运算与闭运算
```python
kernel = np.ones((30,30), np.uint8)
# 开运算：先腐蚀，再膨胀
opening = cv2.morphologyEx(img, cv2.MORPH_OPEN, kernel)

# 闭运算： 先膨胀，再腐蚀
closing = cv2.morphologyEx(img, cv2.MORPH_CLOSE, kernel)

```

## 梯度运算
> 梯度 = 膨胀 - 腐蚀
```python
gradient = cv2.morphologyEx(img, cv2.MORPH_GRADIENT, kernel)

```


## 礼帽和黑帽
> 礼帽 = 原始输入 - 开运算
> 黑帽 = 闭运算 - 原始输入

```python
# 礼帽
tophat = cv2.morphology(img, cv2.MORPH_TOPHAT, kernel)
# 黑帽
blaskhat = cv2.morphology(img, cv2.MORPH_BLACKHAT, kernel)

```

# 图像梯度
## Sobel算子
$$
\left[\mathbf{G}_{x}=\left[\begin{array}{ccc}
-1 & 0 & +1 \\
-2 & 0 & +2 \\
-1 & 0 & +1
\end{array}\right] * \mathbf{A} \text { and } \quad \mathbf{G}_{y}=\left[\begin{array}{ccc}
-1 & -2 & -1 \\
0 & 0 & 0 \\
+1 & +2 & +1
\end{array}\right] * \mathbf{A}\right.$$


```python
dst = cv2.Sobel(src,ddepth,dx,dyksize)
```
- ddepth：图像的深度
- dx和dy分别表示水平和竖直方向
- ksize是Sobel算子的大小
```python
sobelx = cv2.Sobel(img, cv2.CV_64F,1,0,ksize=3)
```
> 当图像从亮色到暗色，图像会显示负值，当图像从暗色到亮色会显示一个正值，正常显示,所以需要求绝对值

```python
sobelx = cv2.converScaleAbs(sobelx)
```
> 计算完成$G_x$, $G_y$后在计算一个整和
$$G = \sqrt{G_x^{2} + G_y^{2} } $$
```python
sobelx = cv2.Sobel(img, cv2.CV_64F,1,0,ksize=3)
sobelx = cv2.converScaleAbs(sobelx)
sobely = cv2.Sobel(img, cv2.CV_64F,0,1,ksize=3)
sobely = cv2.converScaleAbs(sobely)
sobelxy = cv2.addWeighted(sobelx,0.5,sobely,0.5,0)
```


## Scharr算子
$$\mathbf{G}_{x}=\left[\begin{array}{lll}
-3 & 0 & 3 \\
-10 & 0 & 10 \\
-3 & 0 & 3
\end{array}\right] * \mathbf{A} \quad \text { and } \quad \mathbf{G}_{y}=\left[\begin{array}{ccc}
-3 & -10 & -3 \\
0 & 0 & 0 \\
-3 & -10 & -3
\end{array}\right] * \mathbf{A}$$
```python
scharrx = cv2.Scharr(img, cv2.CV_64F,1,0,ksize=3)
scharrx = cv2.converScaleAbs(scharrx)
scharry = cv2.Scharr(img, cv2.CV_64F,0,1,ksize=3)
scharry = cv2.converScaleAbs(scharry)
scharrxy = cv2.addWeighted(scharrx,0.5,scharry,0.5,0)
```

## laplacian算子
$$G = 
\begin{bmatrix}
 0 & 1 & 0\\
 1 & -4 & 1\\
 0 & 1 & 0
\end{bmatrix}$$

```python
laplacian = cv2.Laplacian(img, cv2.CV_64F,1,0,ksize=3)
laplacian = cv2.converScaleAbs(laplacian)
```


# 边缘检测
## Canny边缘检测
1. 使用高斯滤波器，以平滑图像，消除噪点
2. 计算图像中每个像素点的梯度强度和方向
3. 应用非极大值抑制，以消除边缘检测带来的杂散响应
4. 应用双阈值检测来确定真实的和潜在的边缘
5. 通过抑制孤立的弱边缘最终完成边缘检测

### 高斯滤波器
$$H = \begin{bmatrix}
 0.0924 & 0.1192 & 0.0924\\
 0.1192 & 0.1538 & 0.1192\\
 0.0924 & 0.1192 & 0.0924
\end{bmatrix}$$

$$
e = H*A = \begin{bmatrix}
 h_11 & h_12 & h_13\\
 h_21 & h_22 & h_23\\
 h_31 & h_32 & h_33
\end{bmatrix}
*
\begin{bmatrix}
 a & b & c\\
 d & e & f\\
 g & h & i
\end{bmatrix}
=
sum
\begin{bmatrix}
 a\times h_11 & b \times  h_12 & c \times  h_13\\
 d \times   h_21 & e \times  h_22 & f \times  h_23\\
 g \times  h_31 & h \times  h_32 &i \times   h_33
\end{bmatrix}$$


### 梯度和方向
$$
\left[\mathbf{G}_{x}=\left[\begin{array}{ccc}
-1 & 0 & +1 \\
-2 & 0 & +2 \\
-1 & 0 & +1
\end{array}\right] * \mathbf{A} \text { and } \quad \mathbf{G}_{y}=\left[\begin{array}{ccc}
-1 & -2 & -1 \\
0 & 0 & 0 \\
+1 & +2 & +1
\end{array}\right] * \mathbf{A}\right.$$
$$G = \sqrt{G_x^{2} + G_y^{2} } $$
$$\theta  = \arctan(G_y/G_x)$$


### 非极大值抑制
1. 线性插值法： 设g1的梯度幅度值M（g1），g2的梯度幅度值M（g2），则dtmp1可以得到`M(dtmp1) = w*M(g2) + (1-w)*M(g1)`,其中w = distance(dtmp1,g2)/distance(g1,g2), distance(g1,g2)表示两点间距离
2. 将像素点周围点分为八个方向，这样只计算前后



### 双阈值检测
1. 设定maxVal和minVal
2. 大于max保留
3. 小于min舍弃
4. 两者之间的有链接到max上的保留，全部都在其中的舍弃




```python
imd = cv2.Canny(img,50,100)
```

## 图像金字塔
### 高斯金字塔：向下取样（缩小）
1. 将$G_i$与高斯内核卷积
2. 将所有的偶数行和列去掉
```python
down = cv2.pyrDown(img)
```

### 高斯金字塔： 向上取样（放大）
1. 将图像在每个方向kuo大卫原来的两倍，新增的行和列用0填充
2. shi用先前的内核（*4）与放大后的图像卷积获取取近似值

```python
up = cv2.pyrUp(img)
```

### 拉普拉斯金字塔

$$L_i = G_i - pytUp(pytDown(G_i))$$
## 轮廓

### 图像轮廓
`cv2.findContours(img, mode, method)`
mode：轮廓检索模式
- RETR_EXTERNAL:只检测最外部轮廓
- RETR_LIST：检索所有轮廓，并保存到一条链表中
- RETR_CCOMP：检索所有的轮廓，bi能够将他们组织成两层：顶层是各部分的外部边界第二层是空洞的边界
- RETR_TREE：检索所有的轮廓，并重构嵌套轮廓的整个层次

method: 轮廓逼近方法 
1. CHAIN_APPROX_NONE: 以freeman链码的方式输出轮廓，所有其他方法输出多边形（顶点的序列）
2. CHAIN_APPROX_SIMPLE： 压缩水平的、竖直的和斜的部分，也就是，函数只保留他们终点的位置

```python
# 图像转换成二值图像
img = cv2.imread('002.jpg')
img1 = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
showimages('asd', img1)
# 转换成二值图像
ret, thresh = cv2.threshold(img1, 127,255,cv2.THRESH_BINARY)
showimages('bit', thresh)
# 原图，轮廓点， 层级
contours, hierarchy = cv2.findContours(thresh, cv2.RETR_TREE, cv2.CHAIN_APPROX_NONE)
# 传入绘制图像，轮廓，轮廓索引，颜色模式。线条厚度
# 需要拷贝，默认会修改原图并保存
copy_img = img.copy()
res = cv2.drawContours(copy_img, contours, -1, (0,0,255),2)
showimages('asdsa', copy_img)
```

### 轮廓特征

```py
# 将轮廓取出
cnt = contours[0]
# 面积
cv2.contourArea(cnt)
# 周长,True表示闭合
cv2.arcLength(cnt, True)
```

### 轮廓近似
```py
img = cv2.imread('002.jpg')
img1 = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
ret, thresh = cv2.threshold(img1, 127,255,cv2.THRESH_BINARY)
contours, hierarchy = cv2.findContours(thresh, cv2.RETR_TREE, cv2.CHAIN_APPROX_NONE)
copy_img = img.copy()
cnt = contours[0]
res = cv2.drawContours(copy_img, [cnt], -1, (0,0,255),2)
showimages('asdsa', copy_img)
epsilon = 0.05*cv2.arcLength(cnt,True)
approx = cv2.approxPolyDP(cnt, epsilon, True)
res = cv2.drawContours(copy_img, [approx], -1, (0,0,255),2)
showimages('asdsa', copy_img)
```



### 边界矩形
```py
x,y,w,h = cv2.boundingRect(cnt)
img4 = cv2.rectangle(img, (x,y), (x+w, y+h), (0,255,0),2)
showimages('asdsa', img4)

```

### 外接圆
```
(x,y),radius = cv2.minEnclosingCircle(cnt)
center = (int(x), int(y))
radius = int(radius)
img6 = cv2.circle(copy_img, center, radius, (0,255,0),2)

```
## 模板匹配
> 模板匹配和卷积原理相似，模板在原图上从原点开始滑动，计算模板与（图像覆盖区域）的差别程度，这个差别方程的计算方法在opencv中有六种，将计算结果放置到矩阵中，作为结果输出，假设源图像大小AxB，模板大小axb，输出矩阵（A-a+1）x (B-b+1)
1. cv2.TM_SQDIFF:计算平方不同，计算的值越小，越相关
2. cv2.TM_CCORR：计算相关性，计算出来的之越大，越相关
3. cv2.TM_CCOEFF：计算相关系数，计算出来的之越大，越相关
4. cv2.TM_SQDIFF_NORMED：计算归一化平方不同，计算出来的值越接近0，越相关
5. cv2.TM_CCORR_NORMED：计算归一化相关性，计算出来的值越接近1，越相关
6. cv2.TM_CCOEFF_NORMED：计算归一化相关系数，计算出来的值越接近1，越相关


```
# template_img,img
res = cv2.matchTemplate(img, template_img, cv2.TM_SQDIFF)
min_val, max_val, min_loc, max_loc = cv2.minMaxLoc(res)
# 多对象匹配，大于多少的都算，遍历res对象


```

## 直方图
`cv2.calcHist(images,channels, mask, histSize, ranges)`
- images: [images]传入
- channels:中括号，0灰度图，[0][1][2]分别对应RGB
- mask:掩模图象
- histSize:BIN数目，使用中括号
- ranges:像素值取值范围（0-255）


### 直方图均衡化
```
# 展示直方图
img.ravel()
plt.hist(img.ravel(), 256)
plt.show
# 均衡化展示
equ = cv2.equalizeHist(img)
plt.hist(equ.ravel(), 256)
plt.show
```


### 自适应直方图均衡化
```
img = cv2.imread('002.jpg', 0)
clahe = cv2.createCLAHE(clipLimit=2, tileGridSize=(8,8))
equ = cv2.equalizeHist(img)
res_clash = clahe.apply(img)
res = np.hstack((img,equ,res_clash))
showimages('asd', res)
```





