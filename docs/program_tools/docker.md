# docker的基本使用

?> 参考资料：

## 一、安装



## 二、启动

启动命令如下：

```shell
systemctl start docker
```

可以通过查看版本，看是否启动成功， 如果启动成功的话，则既有client又有server

```shell
docker version
```

## 三、常用操作

查看镜像，可以用来查看已下载的镜像

```shell
docker images
```

下载镜像，：后面的表示版本，可以去dockerhub上面看有哪些images可以下载

```
docker pull image:latest
```

启动容器

```
docker run image:latest --name image_name -v  -p 80:8080
```

-v 表示挂载的外部目录，因为docker容器其实就相当于一个虚拟机，所以虚拟机自己里面有对应的目录，而我们可以让外部和虚拟机的某个目录实现共享，方便操作

-p 表示端口的映射

当然也是可以直接进入到启动的容器里面的，通过下面的指令就可以实现

```
docker exec -it 容器id bash
```

这样就可以直接进入到容器里面，这里以MySQL容器举例

![image-20220521181529771](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220521181529771.png)

我们在外部是不存在mysql，可以尝试在主机执行

```shell
mysql -uroot -p
```

![image-20220521181640016](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220521181640016.png)

可以看到，根本就没有mysql这个命令，因为我们的物理机是没有mysql环境的，但我们在容器内，也就是我们的虚拟机中，则是存在mysql的

![image-20220521181728516](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220521181728516.png)

要退出容器的话，直接exit就可以了。

如果要想查看当前有哪些容器正在运行中，可以执行下面的命令

```shell
docker ps
```

如果想要查看所有的已经存在的容器，则可以在前面的命令后加上-a

```shell
docker ps -a
```

接下来，查看容器id，或者容器名，就可以直接启动对应的容器了

```shell
docker start 容器id/容器名
```

这里可以理解为前面的pull是下载虚拟机镜像，之后的run则相当于安装虚拟机，而现在的start则是让虚拟机开机。

要关机的话，或者说关闭容器，通过stop就可以实现

```shell
docker stop 容器id
```

