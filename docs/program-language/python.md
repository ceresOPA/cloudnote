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

## 流畅的Python

?> 《流畅的Python》笔记，简单记录一些内容，可能会比较零散

### 第一章，数据模型

**知识点1**：可以通过简单地实现\_\_getitem\_\_和\_\_len\_\_这两个特殊函数(magic function)来达到python中list、tuple这样的内置数据类型的类似操作（包括下标索引、切片、排序等操作）。

```python
# 非书中例子，这里用一个更简单的例子来说明如何通过__getitem__和__len__实现类似list的下标查找、切片、以及排序操作
import collections

# 通过namedtuple创建一个类（该类仅有属性，无函数）
Human = collections.namedtuple("Human",["name","age"])

class Humans:
    def __init__(self):
        h1 = Human("ZhangSan",17)
        h2 = Human("LiSi",18)
        h3 = Human("XiaoMing",12)

        self.humans = [h1,h2,h3]
    
    def __len__(self):
        return len(self.humans)
    
    def __getitem__(self,idx):
        return self.humans[idx]



h = Humans()

print("下标索引",h[0])
print("切片",h[:2]) 
print("按年龄排序",sorted(h,key=lambda x:x.age))
```

输出结果：

```shell
下标索引 Human(name='ZhangSan', age=17)
切片 [Human(name='ZhangSan', age=17), Human(name='LiSi', age=18)]
按年龄排序 [Human(name='XiaoMing', age=12), Human(name='ZhangSan', age=17), Human(name='LiSi', age=18)]
```

同样的，使用for...in...进行迭代也是可以的

```python
for x in h:
	print(x)

for x in reversed(h):  # 包括内置函数reversed也是可以调用的
	print("reversed",x)
```

**知识点2**：特殊方法的存在是为了被python解释器调用的，而不是自己显式地去调用

- 如，不会使用my_object.\_\_len\_\_()，而是使用len(my_object)
- 对于自定义类的对象，python会自动去调用\_\_len\_\_（前提是实现了该函数），而对于python内置的类型（list、str、bytearray），Cpython解释器则会直接返回PyVarObject中的ob_size属性（PyVarObject是），这会比调用\_\_len\_\_来说更快

**知识点3**：特殊方法的存在，让我们能够让自定义的类的对象模拟内置类型的操作，从而让我们写出更具表达力的代码。

```python
# 自定义Vector类

from math import hypot  # sqrt(x*x+y*y)

class Vector:
    def __init__(self,x=0,y=0):
        self.x = x
        self.y = y
    
    def __repr__(self):
        return "Vector(%r,%r)"%(self.x,self.y)
    
    def __abs__(self):
        return hypot(self.x,self.y)

    def __add__(self,other):
        x = self.x + other.x
        y = self.y + other.y

        return Vector(x,y)
    
    def __mul__(self,scaler):
        x = self.x*scaler
        y = self.y*scaler

        return Vector(x,y)

v1 = Vector(3,4)
print(v1)
print("向量的模",abs(v1))
v2 = v1*3
print("向量的拉伸",v2)
print("向量相加",v1+v2)
```

输出的结果：

```shell
Vector(3,4)
向量的模 5.0
向量的拉伸 Vector(9,12)
向量相加 Vector(12,16)
```

**知识点4**：\_\_str\_\_和\_\_repr\_\_方法的区别

- str方法的触发时机是转换为字符串的时候，如print打印，或str.format时。
- str方法主要用于用户的查看，具有明确的可解释性，而repr主要对于开发者，用于方便调试，当实现了repr后，对象的显示就不会是地址，而是repr的实现内容。
- 当str方法没有实现时，则会去找repr方法，因此可以优先实现repr方法。



###  第二章，序列构成的数组

**知识点1**：深入理解Python中的不同序列类型，不但能让我们避免重复造轮子，而且他们的API还能帮助我们把自己定义的API设计得和原生的序列一样，或者是跟未来可能出现的序列类型保持兼容。



**知识点2**：内置序列类型

容器类型与扁平类型

容器类型：list、tuple、collections.deque，容器类型存放的只是各个元素的引用，因此可以存放不同的类型的数据

扁平类型：str、bytes、array.array、memoryview、bytearray，扁平类型是直接存放的字符、字节或数值，是连续存放的，因此更加紧凑，但是只能存放相同的数据类型的元素

可变序列与不可变序列

可变序列：list、collections.deque、array.array、bytearray、memoryview

不可变序列：tuple、str、bytes



**知识点3**：列表递推式(list comprehension)和生成器表达式(generator expression)

- 列表递推式和生成器表达式在写法上的唯一区别就是前者用[]，后者用()
- 二者的不同在于，如果要直接创建一个list，当然是直接列表递推式



**知识点4**：虽然python让我们能够通过+和*，就能够实现序列的拼接，但是在对于容器序列的时候，在使用上也是要注意，像list中存放的是引用这个问题

```python
a = [['_']*3]*3
"""
打印a:
[['_', '_', '_'], ['_', '_', '_'], ['_', '_', '_']]
"""
a[0][1] = 'X'
"""
打印a:
[['_', 'X', '_'], ['_', 'X', '_'], ['_', 'X', '_']]
"""
```

这里会发现明明只是修改了a\[0\]\[1\]的值，却把a\[1\]\[1\]和a\[2\]\[1\]也给修改了，这是因为其实这里面的三个子list，因为都是*3创建的，因此都是指向的同一个引用，也就是同一个对象，修改其中一个，当然都会变了。

?> PS：这里我一开始还有点疑惑的地方是，不是说list里面是引用吗？那我的 \['_'\]*3 ，这个不也是相同的对象吗？确实是同一个，我用id(a\[0\]\[0\])、id(a\[0\]\[1\])，也都是验证过了是同一个地址，**但是**，如果我改变了如a\[0\]\[1\]，他并不是在原有的对象去修改，而是去创建了一个新的对象，再赋值回来了，所以这也就是为什么虽然id(a\[0\]\[0\])、id(a\[0\]\[1\])一开始是同一个地址，但对其中一个修改后另一个却没有发生变化的原因。至于为什么是创一个新的对象，而不是在原有的对象上修改，这又要回到list是可变序列，而str是不可变序列的问题上了。

那如果想要正确的创建上面的二维数组，那应该怎么创建？为了简洁的话，可以通过列表推导式来创建，当然用for循环，每次创建一个新对象去添加也是可以的，关键其实就是在于是否是同一个对象的引用。

```python
a = [['_','_','_'] for i in range(3)]
"""
打印a:
[['_', '_', '_'], ['_', '_', '_'], ['_', '_', '_']]
"""
a[0][1] = 'X'
"""
打印a：
[['_', 'X', '_'], ['_', '_', '_'], ['_', '_', '_']]
"""
```

**知识点5**：python中的省略号(...)和多维切片

省略号

- ... 其实是ellipsis这个类的唯一的实例Ellipsis的别名（ellipsis是类名，Ellipsis是对象名，这里类似bool是类，False和True是大写的）

- 其实...是三个英文句号，并不是半个省略号

多维切片

- 我们通常使用的切片操作其实都是一维的，如a[2:5]，是拿取下标为2、3、4的这三个元素，但其实方括号[]中，也是可以支持使用逗号（,）的，如[2,3]这样的方式，如果想像这样使用，就需要自己重新实现\_\_getitem\_\_、\_\_setitem\_\_这样的特殊方法（让getitem能够去接收元组，这样就可以很自然地在[]中使用 ","）

对于python中的省略号和多维切片，这样的句法上的特性都体现出了python的高度可扩展性，让我们在原有的基础上去实现自定义的类。不仅是避免了重复造轮子，而且能够让自定义的类和python的内置类实现类似的表达方式，很秒！

上面两个句法的典型例子就是numpy了

```python
a = np.random.rand(2,3,5)
# 打印后会发现a[1,...]和a[1,:,:]是等价的
print(a[1,...])
print(a[1,:,:])

#下面的打印输出后，得到的是大小为(2,2,3)的三维数组，即多维切片
print(a[:,1:,2:5])
```



**知识点6**：元组既可以被用作无字段名的记录，也可以被用作不可变的list

- 对于元组在作为记录时，如("XiaoMing","HuNan",23)，这样的一条记录表示的就是(Name,Province,Age)。
- 在元组中，我们通常还会用元组拆包（tuple package），



```python
t = (1,2,[3,5])
t[2]+=[6,8]
print(t)
```



### 第三章，字典和集合



知识点1：UserDict是dict的python实现，用于用户自定义类的时候需要继承dict，则使用UserDict

下面的链接说明了为什么明明有dict了，却还是存在UserDict，甚至包括UserString这些类的原因

知乎链接：https://zhuanlan.zhihu.com/p/25915483

(C语言实现的dict会忽视重载的特殊方法)

UserDict并非继承自dict，dict在某些实现上会走一些捷径，会去忽视掉重写的特殊方法。UserDict在很多地方已经为我们实现好了，可以进行更加简洁的操作，而不用去重写过多的方法。





>  鸭子类型：只要长得像鸭子，走的像鸭子，叫的像鸭子，那就可以认为是鸭子

动态类型的一种风格，不用非是继承自同一类的子类，只要是有类似的方法或属性，就可以认为是同一类





## 算法

### DFS（深度优先遍历）

![dfs](https://gitee.com/y255413580/img/raw/master/noteimg/dfs.png)

```python
'''
递归，相当于树的先序遍历
'''
def dfs(graph,start,path):
    if(start not in path):
        path.append(start)
    for k in graph[start]:
        if(k not in path):
            dfs(graph,k,path)
    return path

if __name__ == "__main__":
    graph = {
        "a":["b","c","d"],
        "b":["c","e"],
        "c":["d"],
        "d":["b","e"],
        "e":[]
    }
    path = []
    path = dfs(graph,"a",path)
    for node in set(graph.keys())-set(path):
        path = dfs(graph,node,path)

    print(f"path:{path}")
```

由于Python中内置的dict、list以及set类型，所以实现起来非常方便，其中图的表示方式，个人认为是类似邻接表的，只是以前我们在c语言的实现中，还要自己使用指针，顶点用数组，然后边用结点表示，然后在Python里直接用了dict数据类型，十分方便。对于深度优先算法的思想，其实非常简单，只需要一直往下走就可以了，唯一需要**注意**的是，**从某些顶点出发并不一定能够经过所有的顶点，因此在无法进行遍历某些顶点时，需要重新选择起点！**这里借助了set集合，能够非常简洁地找到未能遍历的顶点。

### BFS（广度优先算法）

这里还是使用上面dfs的图

```python
'''
队列，相当于树的层次遍历
'''
from utils import Queue

def bfs(graph,start,path):
    q = Queue()
    if(start not in path):
        path.append(start)
        q.insert(start)
    while(q.len()>0):
        tmp = q.pop()
        for e in graph[tmp]:
            if(e not in path):
                path.append(e)
                q.insert(e)
    return path

if __name__=="__main__":
    graph = {
        "a":["b","c","d"],
        "b":["c","e"],
        "c":["d"],
        "d":["b","e"],
        "e":[]
    }
    path = bfs(graph,"c",[])
    for node in set(graph.keys())-set(path):
        path = bfs(graph,node,path)

    print(f"path:{path}")
```





## 其他

### Vscode无法在Anaconda环境的Python下执行代码

解决方案：

[Vscode中报错 CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'. - 简书 (jianshu.com)](https://www.jianshu.com/p/f5338df30470)

![image-20220218191728455](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220218191728455.png)