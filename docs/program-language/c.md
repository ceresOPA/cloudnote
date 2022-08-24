# C语言



## 笨办法学C语言

> 学习地址：https://wizardforcel.gitbooks.io/lcthw/content/ex1.html

### 练习0：准备

我采用了Windows的子系统Ubuntu，安装方法：

步骤一、在**程序与功能**中启用 **适用于Linux的Windows子系统**

步骤二、打开微软应用商店，搜索下载Ubuntu即可

```shell
$ sudo apt-get install build-essential
```

这里执行命令后，出现了 Failed to fetch 404 not found 的问题，解决方法为：

步骤一、输入命令

```shell
$ sudo vi /etc/resolv.conf
```

步骤二、在resolv.conf中添加阿里DNS

```
nameserver 223.5.5.5
nameserver 223.6.6.6
```

步骤三、保存后，执行完下面的命令，就可以重新安装自己的需要的包了

```shell
$ sudo apt-get update
```

### 练习1：启用编译器

```
make hello
```



```
gcc hello.c -o hello
```





## 问题篇

### 1. i++和++i哪个更快？

- ++i更快
- i++可以认为是y = i; i = i+1;两条语句
- ++i可以认为是y = i+1;一条语句

### 2. 结构体的 . 和 -> 的区别

- 定义的若为一个结构体数据类型的变量，那就会是通过 . 来访问成员变量
- ->的话，则是定义的为变量的指针
- 如，(*p).param 等价于 p->param

### 3. static的作用

- static作用于局部变量上，局部变量加上static后，改变了其生命周期，让它能够在函数调用结束后，仍然存在，其存放的位置为全局数据区，而普通的局部变量则是放在栈中，调用结束就释放。
- static作用于全局变量上，改变了作用域，只在本文件中发挥作用。
- static作用于函数上，和作用于全局变量类似，改变了其作用域。

### 4. strcpy sprintf memcpy区别

- strcpy的参数是两个字符串类型，实现是字符串的复制。
- sprintf是将其他类型转换为字符串类型，效率最低。
- memcpy则不局限于字符串，可以是任何数据类型，因为他是直接作用于地址空间，速度最快。

### 5. C语言的类型

- 基本数据类型，包括整型和浮点型，整型有int，short，long，char字符类型，浮点型有float和double。
- 派生类型，有指针、函数、结构体、共用体。
- 枚举类型，enum，用来简化代码，增强可读性。

### 6.野指针

- 产生原因：
  - 声明指针的时候，没有进行初始化，指针指向了一个随机的地址
  - 释放指针所指向的地址空间后，没有让指针指向NULL，指针指向了一个无效的地址
  - **指针操作超越变量作用域：不要返回指向栈内存的指针或者引用，因为栈内存在函数结束的时候会被释放。**
- 解决方法：
  - 直接给指针赋值为NULL

### 7.结构体与共用体

- 需要注意的是共用体是共用内存的，使用sizeof返回的大小是最大的成员变量的长度，如下面的程序：

```c
#include <stdio.h>
 
union Data
{
   int i;
   float f;
   char  u[20];
} data;
 
int main( )
{
   union Data data;        
 
   printf( "%d\n", sizeof(data));
 
   return 0;
}
```

输出的大小为20，也就是char u[20]的大小。

### 8. 内联函数

```c++
#include <iostream>
 
using namespace std;

inline int Max(int x, int y)
{
   return (x > y)? x : y;
}

// 程序的主函数
int main( )
{

   cout << "Max (20,10): " << Max(20,10) << endl;
   cout << "Max (0,200): " << Max(0,200) << endl;
   cout << "Max (100,1010): " << Max(100,1010) << endl;
   return 0;
}
```

内联函数的使用和普通函数类似，不同的是普通函数是通过调用的方式执行的，而内联函数是直接用函数体替换调用的表达式，速度一般会更快，可以认为是一个空间换时间的策略，因为内联函数不需要再经过压栈出栈的调用过程。但如果大量使用内联函数会增大代码的体积，反而会导致性能下降，所以一般只对10行以内的小函数使用内联函数。

### 9.函数指针和指针函数

- 函数指针，就是指向函数的指针
- 指针函数，指的是函数的返回为指针类型

### 10. 数组指针和指针数组

- 数组指针，即指向数组的指针 ，如 ，int (*p)[10]
- 指针数组，即元素均为指针的数组，如 ，int *p[10]