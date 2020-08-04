---
title: C11关键字_Static_assert
comments: true
copyright: true
date: 2020-02-24 20:31:58
updated:
tags: C11 关键字 _Static_assert
categories: C
keywords: C11 关键字 _Static_assert
description: C11关键字_Static_assert介绍
top_img: https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/C语言中输入输出所有格式控制符/figure1.jpg
cover: https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/C语言中输入输出所有格式控制符/figure1.jpg
toc: true
toc_number: true
---

# C11新增关键字\_Static\_assert

&emsp;C11标准新增了一个关键字\_Static\_assert,下面来介绍一下它的相关用法和注意事项。

## 用法

&emsp;和C11以前的断言assert\(\)表达式有些类似。assert\(\)表达式是在运行时检查。而C11新增的\_Static\_assert声明可以在编译时检查assert\(\)表达式。因此，assert\(\)可以导致正在运行的函数中止，而\_Static\_assert()可以在导致程序无法通过编译。\_Static\_assert()接受两个参数。第一个参数是一个整型常量表达式，第二个参数是一个字符串。如果第一个表达式求值为0（或\_False）,编译会显示字符串，而且不编译该程序。

## 例程

下面是一个使用\_Static\_assert的例程：

```C
#include <stdio.h>
#include <limits.h>

_Static_assert(CHAR_BIT == 16, "16-bit char falsely assumed");
int main(void)
{
    puts("char is 16 bits.");
    return 0;
}

```
&emsp;编译结果

```C
statasrt.c:4:1:Error:static assertion failed: "16-bit char falsely assumed"
_Static_assert(CHAR_BIT == 16, "16-bit char falsely assumed");
^              ~~~~~~~~~~~~~~~~~~~~~~~
1 error generated.

```

## 注意事项

- 性能方面：assert()是运行期断言，它用来发现运行期间的错误，不能提前到编译期发现错误，也不具有强制性，也谈不上改善编译信息的可读性。既然是运行期检查，对性能肯定是有影响的，所以经常在发行版本中，assert都会被关掉。由于static_assert是编译期间断言，不生成目标代码，因此static_assert不会造成任何运行期性能损失。
- 使用范围：static_assert可以用在全局作用域中，命名空间中，类作用域中，函数作用域中，几乎可以不受限制的使用。
- 常量表达式：static_assert的断言表达式的结果必须是在编译时期可以计算的表达式，即必须是常量表达式。如果使用变量会导致错误。


