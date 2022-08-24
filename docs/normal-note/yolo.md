# YoloV5

> 学习资料：https://space.bilibili.com/237133596?spm_id_from=333.337.0.0   up主：薛定谔的AI

## 原理篇

### 一、数据增强

#### 1. rectangular

同个batch里做rectangle宽高等比变换，加快训练

通常一般会给所有的图片resize为同一个大小，但是这样可能会造成某些图片的黑边会比较多，加大了计算，所以，在yolov5中，采用了在batch中都设置一个最合适这个batch中图片的size。

为什么会有黑边？

因为要不影响图片的分辨率，为了能够进行等比例的缩放，那就需要进行填充后再进行resize，这样也就避免不了会有黑边的存在。

#### 2. 图片的旋转缩放

<img src="https://gitee.com/y255413580/img/raw/master/noteimg/image-20220317165844568.png" alt="image-20220317165844568" style="zoom:50%;" />

旋转缩放矩阵实现

#### 3. 平移



#### 4. 错切/非垂直投影

![image-20220317170835276](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220317170835276.png)

#### 5. 透视变换

![image-20220317171159804](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220317171159804.png)

透视变换，就是指我们从不同的方向看过去的一个正视的图像，可以看到右侧的这个图像，可以看作是从右侧看过去的一个角度。通常在OCR识别中都会需要选择四个角，然后进行一个透视变换从而将图片转换为一个正视着的图像。

#### 4. 图像翻转

- 上下翻转
- 左右翻转

![image-20220317172413961](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220317172413961.png)

#### 图像拼接

![image-20220317172958417](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220317172958417.png)

#### 图像融合

![image-20220317173225495](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220317173225495.png)

#### 分割填补

![image-20220317173301362](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220317173301362.png)

#### 完成图像本身的变换后，也不要忘了坐标的变换

![image-20220317173835121](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220317173835121.png)