# picgo

picgo，可以说是和typora的完美搭配组合，能够方便地将笔记中的图片上传到图床中，这样可以方便在不同设备上查看，如果有个人网站的，那就更不用说了。

## 一、安装

安装没什么好说的，这里贴一下github 的地址：[PicGo](https://github.com/Molunerfinn/PicGo)

安装完成之后就是要进行图床的配置了，这里先贴一下要进行的配置：

![image-20220525155330577](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220525155330577.png)

## 二、配置

这里以前使用的gitee的，但后面挂掉了，gitee真的一言难尽。所以没有办法，后面使用github配合jsDelivr的cdn加速，也还是挺不错的，下面写出配置流程，方便以后查看。

### 1. 新建github图床仓库

这里直接在github上new一个仓库就可以了，可以的话，还可以加个文件夹，我这里是加了个img的文件夹。

![image-20220525154934965](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220525154934965.png)

### 2. 获取Token

token相当于就是一个用户凭证，因为不可能随便让别人上传文件到你自己的仓库吧，所以当然是需要有一个token来相当于一个自己身份的认证了，这里点击右上角自己的头像，然后点击setting，就可以进入我下面这里的界面。

![image-20220525155552465](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220525155552465.png)

接下来点击这个界面最下方的develop settings就可以进入到我们要获取token的界面了

![image-20220525155655516](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220525155655516.png)



![image-20220525160231371](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220525160231371.png)

### 3. PicGo中GitHub仓库配置

接下来就是最后的picgo配置一步了，仓库名就是你自己的 用户名/仓库名，分支名，这里我们才刚创建，所以是默认的，要么是master要么main，不过也可以直接去仓库那里看一眼是什么就好。然后关于token就是我们前面从GitHub上获取的那个token，指定存储路径，这里我是在仓库还创建了一个img文件夹，因此这里直接指定到了这个文件夹下。最后，也就是这个自定义域名的设置，这个还挺关键的，因为我们需要进行cdn加速，因此这里不使用默认的github的访问路径，这里直接替换一下就行。

https://cdn.jsdelivr.net/gh/你的用户名/你的仓库名@发布的版本号/文件路径

![image-20220525161125883](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220525161125883.png)

## 三、 typora配置PicGo

选择 文件 => 偏好设置 => 图像 => 设置PicGo路径

![image-20220525163331925](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220525163331925.png)

