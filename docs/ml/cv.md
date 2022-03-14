# 计算机视觉

## The Goal of Computer Vision

- 视觉鸿沟
- To bridge the gab between pixel and “meaning”

## Processing

- Computational theory ： 干什么
- Representation and algorithm ：怎么表示，怎么去算
- Hardware implementation ：应用

## 卷积

### Type of Image

#### 二进制图像

- 值要么是0，要么是1

#### 灰度图像

- 取值范围为0~255

#### 彩色图像

- 三通道
- 每个通道中的像素取值也是0~255

### Motivation : Image Noise（去噪）

#### Moving Average

- 平均像素点，加权平均
- 这些权值叫卷积核（或滤波核）

#### Define convolution

- 卷积核翻转？？flipped？
- $\sum f([m-k,n-l])*g(k,l)$，如，模板大小为3X3，k,l取值-1，0，1，（m，n）是当前的像素点

### Practice with Linear filters

- 卷积可以用来进行平移
- 卷积可以让图像平滑化
- 卷积可以实现图像锐化
- ![image-20220314082215022](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220314082215022.png)

### 平滑问题

![image-20220314082136919](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220314082136919.png)

- 振铃现象，出现不期望的原本不存在的点

### Choose kernel width

![image-20220314083604859](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220314083604859.png)

设置为$3 \sigma$后，基本上可以把周围的像素点都囊括（如果保持$\sigma$不变，不断增大模板窗口大小，那外面的权值反正都为0，加再大也没有意义），即概率接近于1，更有利于之后进行归一化操作（为什么要归一化？因为要让像素点的值在0~255之间，如果加起来的权值不到1，那最大值肯定也到不了255）。

### 高斯核

- 滤去高频，保留低频，滤波去噪
- 小高斯核可以构成大的高斯核（加速运算，小高斯核的运算肯定要比大高斯核要简单）
- ![image-20220314093305397](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220314093305397.png)
- ![image-20220314093615155](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220314093615155.png)

### Noise

- 椒盐噪声（既有白点又有黑点）
- 脉冲噪声（全是白点）
- 高斯噪声（当高斯噪声的方差大的时候，就可以用方差更大的高斯核来去噪）

### 高斯滤波

- 高斯滤波虽然能够去噪，但是会让边缘退化，让边缘没那么明显

### Median Filter

- 中值滤波
- 将模板内的像素点的值进行排序，选择中间的那个值，是一种非线性的滤波器
- 能够很好地解决椒盐噪声、白噪声的问题，因为椒盐噪声中很多都是些突兀的像素点，那么通过取中值的话，就能够让他们的像素点相同，而不会被这些噪声点所影响

?> 一般把高斯滤波叫作平滑滤波，同时注意卷积也并不只是可以做平滑，也还可以进行平移。

### 锐化

![image-20220314095920503](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220314095920503.png)

## 边缘提取

- 边能够紧凑地表达图像的信息

### 边缘特征

#### 如何找信号突变的地方？

- 求导，找导数值最大的点，也就是突变最剧烈的点

- ![image-20220314100916878](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220314100916878.png)

- ![image-20220314101325418](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220314101325418.png)

- ![image-20220314101849305](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220314101849305.png)

- 梯度强度、梯度方向与边的方向垂直