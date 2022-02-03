# Python

?> 学习资料：<br/>[Kaggle Course](https://www.kaggle.com/learn)<br/>强烈建议去看一下kaggle的这个Python教程，而且也有练习，下面python基础部分只是一个简单的总结。

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

### Step7. 认识Boolean

Boolean类型的值只有True和False，我们在进行大小比较、条件判断的时候都会间接地用到布尔类型。

```python
2>3 #False
2.0 == 2 #True
"2" == 2 #False
```

当然，和其他数据类型一样，也是可以直接对变量赋值为boolean类型的。

```python
x = False
"""
>>>type(x)
<class "bool">
"""
```

和运算操作符一样，也有布尔操作符，and、or和not。同样的，这三个也有默认的顺序，not>and>or，但一般来说为了代码的可读性，最好还是自己加好括号。

```python
True and (False or True) # True
```

### Step8. 使用条件判断

认识了Boolean后，与其密切相关的就是条件判断，之前的例子中也有提及。

```python
if(x>0):
	print(x,"is a positive number.")
elif(x<0):
	print(x,"is a negative number.")
elif(x==0):
	print(x,"is zero.")
else:
	print(x,"seems not a number.")
```

除了上面常规的写法，在一些简单的条件判断中我们还可以使用更加简洁的条件表达式：

```python
a = 2
b = 3
max = a if a>b else b #会得到max=3
```

?> 额外内容：前面在这一步布尔与条件的kaggle教程中，最后有一个要自己写21点这个游戏策略来取得胜利的函数，玩的有点上头，只能想到一些简单的条件判断，然后返回抓不抓牌，搞了半天也只能拿到39%的胜率，不知道他那个AI是怎么设置的，或者难道是训练出来的？不是简单的逻辑判断？搞不懂，但挺有意思的，可以去玩一下。<br />      有点跑偏了，这里其实想说的是**bool类型转int型的妙用**,也很有意思。

```python
int(True) #输出1
int(False) #输出2
int(True)+int(False)+int(True) #显然会输出2，但其实并不需要自己显式地转换，python在处理bool的加法时会默认进行转换的
True+False+False #输出1
```

通过上面的介绍，可以应用到一个小例子中，比如，

题目：只有当盘子里只有苹果、香蕉和橘子中的其中一种水果的时候我才吃，即没有水果我肯定不吃，如果有多种水果我也不吃，那么现在写一个函数，返回True或False，True表示我吃，False表示我不吃。

```python
def isEat(apple,banana,orange):
    return apple+banana+orange==1
# isEat(True,False,False) 返回True
# isEat(True,True,False) 返回False
# isEat(False,False,False) 返回False
```

可以看到通过bool的加法运算，可以超级方便地实现一些条件的判断，之后应当充分灵活运用这个特性。

### Step9. 认识List

List同样是Python中的一种数据类型，类似其他语言中的数组，但是会有众多Python才有的特性，下面会进行一些介绍。

可以直接通过中括号赋值一个List变量：

```python
ls = [2,3,4,5,6]
ls = ["hello","world"]
ls = ["two",2,abs] #一个list中甚至可以定义不同数据类型的元素，包括function，例如其中的abs函数
ls = [[1,2,3],[4,5,6],[7,8,9]] #list中定义list当然也是可以的
```

定义好一个List后，当然是学会如何去找到List中的元素了，和其他语言的数组类似，可以直接使用下标进行访问：

```python
ls = [2,3,4,5,6]
ls[0]
>>>2
ls[-1] #在python中下标是可以小于0的，小于0就是从右侧往左数
>>>6
```

Python的List除了直接的访问利用下标访问某个元素外，还有更加强大的功能，即可以通过Slice(切片)实现截取List的部分元素：

``` 
ls = [2,3,4,5,6]
ls[1:3] #输出[3,4] 左闭右开(包含左侧下标的元素，不包含右侧下标的元素)
ls[:3] #输出[2,3,4] 左侧不填，默认为0
ls[3:] #输出[5,6] 右侧不填，默认为list的长度,这里为5
ls[1:-1] 输出[3,4,5] 负数也一样可以使用
```

除了可以通过切片截取List外，还可以重新对截取的部分进行赋值：

```python
ls = [2,3,4,5,6]
ls[1:3] = [6,6] #ls就会变为[2,6,6,5,6]
```

学习了List的一些基础之后，可以学习一些与List相关的函数，这里形如ls.index()称之为method(方法)，而x.imag称之为attribute(属性)。

```python
ls = [1,3,2,5,6]
sum(ls) #输出17
sorted(ls) #默认按大小排序[1,2,3,5,6]
```

list相关的方法可以使用help(List)来进行查看，并且也会打印出这些函数的一些使用方法，这里介绍一些常用的方法。

```python
ls = [1,3,2,5,6]
ls.index(2) #输出2，表示元素2的下标为2,只会从左到右，查找的该元素首次找到的下标，如ls后面若还有2，但是仍然是返回下标2
ls.append(3) #无返回值，list变为[1,3,2,5,6]，在最后添加元素
ls.pop() #弹出并返回最后一个元素，ls变为[1,3,2,5,6]
```

### Step10. 认识tuple

tuple元组与List列表类似，需注意的是tuple是immutable(不可变)的，而list是mutable(可变的)

tuple的定义如下：

```python
#方式一
tu = (1,2,3,4,5,6)
#方式二
tu = 1,2,3,4,5,6
```

注意：当试图去修改tuple中的元素的时候会发生报错

### Step11. Loop循环

学习了list和tuple后，就可以通过for循环来实现对list或tuple的遍历。

```python
ls = [1,2,3,4,5,6]
for x in ls:
	print(x,end=' ')
# 打印输出1 2 3 4 5 6
```

其实不止是list或tuple类型的变量，只要是可迭代的对象，就可以使用for循环进行遍历，如str类型的数据也是可以的。

Python中除了可以实现上面的循环语句外，也可以通过range()来实现。

```python
for i in range(10):
    print(i,end=' ')
# 打印输出0 1 2 3 4 5 6 7 8 9
```

### Step12. 列表推导式

列表推导式的使用能够使代码更加简单、方便。

```python
[i for i in range(10) if i%2==0]
# 输出列表[0,2,4,6,8]
```

