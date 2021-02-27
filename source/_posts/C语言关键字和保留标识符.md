---
title: C语言关键字和保留标识符
comments: true
copyright: true
date: 2020-02-24 20:30:39
updated:
tags: C 关键字 保留标识符 
categories: C/C++
keywords: C 关键字 保留标识符
description: C语言中所有的关键字和保留标识符
top_img: https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/C语言中输入输出所有格式控制符/figure1.jpg
cover: https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/C语言中输入输出所有格式控制符/figure1.jpg
toc: true
toc_number: true
---

# C语言关键字和保留标识符

## 关键字

&emsp;关键字是C语言的词汇。它们对于C语言比较特殊，不能用他们作为标识符(如，变量名)。许多关键字用于指定不同的类型。如int。还有一些（如if）语句用于控制程序中语句的执行顺序。下表列出了所有的C语言关键字，包括C11标准中出现的。

|关键字  |标准  |描述|
|:--:   |:--:  |:--|
|auto   |K&R C|表明有意覆盖一个外部变量定义或者强调不要把该变量改为其他存储类别|
|extern |K&R C|显式声明一个定义与其他文件中的外部变量|
|short  |K&R C|声明短整型数据|
|while  |K&R C|程序循环|
|break  |K&R C|跳出循环语句|
|float  |K&R C|声明单精度的浮点数|
|signed |C90   |声明带符号的数据类型|
|\_Alignas|C11  |用于指明内存对齐相关|
|\_Alignof|C11  |指明内存对齐相关| 
|sizeof |K&R C|返回指向的数据对象或类型所占空间的大小，以字节(byte)为单位|
|for    |K&R C|程序循环|
|case   |K&R C|分支语句|
|char   |K&R C|声明单字符型变量|
|goto   |K&R C|程序跳转到指定的位置|
|static |K&R C|声明具有静态存储期的数据|
|\_Atomic|C11   |并发编程时声明一个原子类型的对象|
|const  |C90   |声明一个不能通过赋值，递增或递减来修改的对象|
|if     |K&R C|条件语句|
|struct |K&R C|声明结构体变量|
|\_Bool  |C11   |声明布尔类型变量|
|continue|K&R C|跳出当前循环，开始下一次循环|
|inline |C99   |声明内联类型|
|switch |K&R C|分支语句|
|\_Complex|C11  |声明复数类型|
|default|K&R C|分支语句|
|int    |K&R C|声明整形变量|
|typedef|K&R C|为对象创建别名|
|\_Generic|C11  |实现泛型选择|
|do     |K&R C|循环语句|
|long   |K&R C|声明长整型|
|union  |K&R C|声明联合体变量|
|\_Imaginary|C11|声明虚数对象|
|double |K&R C|声明双精度浮点数|
|register|K&R C|声明寄存器类型的变量|
|unsigned|K&R C|声明无符号类型变量|
|\_Noreturn|C11 |设置调用函数后不返回|
|else   |K&R C|条件语句|
|restrict|K&R C|用于编译器优化，只能用于指针，表明指针是访问数据对象唯一的方式|
|void   |K&R C|声明无类型的变量|
|\_Static\_assert|C11|声明编译时检查的断言|
|enum   |C90   |声明枚举类型|
|return |K&R C|函数返回|
|volitile|C90  |用于编译器优化,告知计算机，代理(不是变量坐所在的程序)可以改变该变量的值|
|\_Thread\_local|C11|声明属于创建线程私有的线程局部数据变量|

&emsp;将所有关键字分一下类大概可以分为如下几类

- 声明数据类型，包括void,int, short,float,signed,unsigned,char,struct,union,\_Bool,\_Complex,long,\_Imaginary,double,enum。  
- 存储类别说明符，包括auto,register,static,extern,Thread\_local,typedef。
- 程序控制，包括while,do,break,case,for,goto,if,continue,switch,default,\_Noreturn,return,else。
- 类型限定，包括const,volatile,restrict,\_Atomic。
- 其他，包括\_Alignas,\_Alignof,sizeof,\_Generic,\_Static\_assert。

## 保留标识符

&emsp;如果使用关键字不当，如用关键字作为变量名，编译器会将其视为语法错误。还有一些保留标识符。C语言已经指定了它们的用途或保留它们的使用权，如果你使用这些标识符来表示其它意思会导致一些问题。因此，尽管他们也是有效的名称，不会引起语法错误，也不能随意使用。保留标识符包括那些以下划线字符开头的标识符和标准库函数名，如printf()。



