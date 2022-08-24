# 动手学深度学习-学习笔记

?> 简单记一下，懒散了。。。

图神经网络（符号学与深度学习的结合，深度学习中也可以进行推理）

```
x[::3,::2]
# 行的步长为3，列的步长为2
```

## 数据操作

通过避免重新创建，节省内存开销

```python
x = torch.arange(3)
y = torch.arange(3)
x += y  # 通过这种方式，x的id不变，也就是x并不是重新创建的，如果通过 x = x+y ，则前后的x的id不同
```

## 数据预处理方法

nan数据的处理

数值类可以填充

```python
x.fillna(x.mean)
```

非数值类的可以进行onehot编码

```python
pd.get_dummies(x)	
```

加快运算的小技巧

```python
x = torch.Tensor([1.2,3.3])
x.dtype # torch.float32  在深度学习中，通常会为了加快计算，会不选择64位的float，而选择32位的float 
```



view和reshape的区别



shape改变的只是样子，还是同一个对象

## 线性代数



## 矩阵运算

亚导数



![image-20220812223053792](C:%5CUsers%5Cy2554%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20220812223053792.png)



