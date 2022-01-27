# Python

?> 学习资料：[Kaggle Course](https://www.kaggle.com/learn)

## python基础

### Step1. 写一个变量

学习一门语言，不可避免地就是要从声明并给变量赋值开始。

不过python特殊在并不需要提前声明，就可以直接进行赋值，并且可以赋予不同类型的数据。

```python
color = 'blue' #单引号和双引号都可以，表示给color赋值为blue这个字符串
color = 2 #赋值为整型int的2
```

### Step2. 调用内置的函数

每门语言中都包含有大量的内置函数，下面演示最简单的打印函数的调用。

```python
print("hello world") #其中括号里面的就是参数，print()就是函数
```

内置的函数还有很多，之前说python中不需要声明变量的类型，但是其实每个变量都是隐含着类型的，这里就可以使用到一个内置函数进行查看。

```python
x = 3
type(x) # 会返回int，也就是说x是int型的变量
x = 1.6
type(x) # 这个时候就会是float，表示浮点型，即有小数点类型的变量
x = 'string'
type(x) # 则会返回str,表示字符串类型,除了这里说的一些常用类型外，还有其他的一些数据类型存在
```

除了有可以查看数据类型的函数外，而且还可以进行数据类型的转换

```python
x = 3.2
int(x)
float(x)
# 可以通过类似int() 、 float()这样的函数对变量进行数据类型的转换
```

### Step3. 进行基本的运算操作

当然，和数学一样，有了能够表示出来的数字，当然是不能少了数值之间的运算了，也就是需要运算符号的参与，这里介绍python中的一些运算操作。

| Operator   | Name           | Description                                            |
| ---------- | -------------- | ------------------------------------------------------ |
| ``a + b``  | Addition       | Sum of ``a`` and ``b``                                 |
| ``a - b``  | Subtraction    | Difference of ``a`` and ``b``                          |
| ``a * b``  | Multiplication | Product of ``a`` and ``b``                             |
| ``a / b``  | True division  | Quotient of ``a`` and ``b``                            |
| ``a // b`` | Floor division | Quotient of ``a`` and ``b``, removing fractional parts |
| ``a % b``  | Modulus        | Integer remainder after division of ``a`` by ``b``     |
| ``a ** b`` | Exponentiation | ``a`` raised to the power of ``b``                     |
| ``-a``     | Negation       | The negative of ``a``                                  |

这些操作和使用计算器是类似的，除了进行一般的数值计算外，python其实也为str类型的数据提供了与运算相关的简单操作，能够更加便捷地使用字符串。

```python
s = "hello"
s = s*5
print(s) # 会输出 hello hello hello hello hello
s = "hello" + " world"
print(s) # 会输出 hello world ,即将两个字符串通过+连接起来
```

### Step4. 变量值的交换

如a = [2,3,4] , b = [5,6,7] ，想要交换a与b的值，即把a的值赋值给a，a的值赋值给b。

这里的一般操作如下：

```python
tmp = a
a = b
b = tmp
```

而在python中可以使用更加简便的方式实现：

```python
a,b = b,a
# 通过这样的一条语句就可以直接实现a与b值的交换
```

### Step5. 查看函数的使用

通过内置函数help()可以非常方便的查看一些函数的使用方法

```python
help(print) #调用之后，就会在控制台打印出print函数可以有哪些参数，以及print函数的返回是什么样的
help(round) #查看round函数的使用
```

注意一下，通过help调用的函数，是不用加括号的，即help(print())这样是不对的。

### Step6. 定义自己的函数

除了能够使用内置的函数外，我们也可以定义自己需要的函数，通过下面的代码就可以实现一个简单的函数。

```python
def getMax(a,b):
	if(a>b):
		return a
	else:
		return b
```

这样就可以通过getMax()来进行调用，如下面这样:

```python
getMax(3,6) # 会返回6，即两者中的最大值
```

一般来说，自己定义好的函数，最好能够写一些说明(docstring):

```python
def getMax(a,b):
    """
    return the maximum number between a and b
    
    parameters
    ----------
    a,int or float
    b,int or float
    
    example
    -----------
    >>>getMax(3,6)
    6
    """
    if(a>b):
        return a
    else:
        return b
```

通过上面写的说明，就可以通过调用help函数，打印出来，既能够方便他人调用，也能够方便自己以后的使用。

可以尝试在控制通过help函数调用一下:

```python
help(getMax)
```

![image-20220127233817786](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220127233817786.png)