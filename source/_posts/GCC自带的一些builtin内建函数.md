---
title: GCC自带的一些builtin内建函数
top_img: 'https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/02/27/GCC自带的一些builtin内建函数/figure1.jpg'
comments: true
cover: 'https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/02/27/GCC自带的一些builtin内建函数/figure1.jpg'
toc: true
toc_number: true
copyright: true
date: 2021-02-27 18:57:00
updated:
tags: 
- GCC
- bulitin函数 
categories: GCC
keywords: GCC 内建函数
description: 一些GCC自带的内建(bulitin)函数的接口及实现
---

## 一、GCC内建函数

&emsp;最近在刷 [leetcode](https://leetcode-cn.com/) 的时候遇到了一些以__builtin开头的函数，它们被用在状态压缩相关的题目中特别有用，于是就去了解了一下。

&emsp;原来这些函数是GCC编译器自带的内建函数。这些__builtin_*形式的内建函数一般是基于不同硬件平台采用专门的硬件指令实现的，因此性能较高。在刷题时可以直接用而不用重复造轮子，尤其是一些涉及到位操作的内建函数真的特别有用。

&emsp;我对其中涉及到位运算的内建函数挺感兴趣的，于是就去查了一下它们的接口以及具体实现。下一节主要介绍一下几个特别有用的函数。

&emsp;关于GCC内建函数的更完整的内容可以参见 [官方文档](https://gcc.gnu.org/onlinedocs/gcc-4.3.2/gcc/Other-Builtins.html#Other-Builtins)。
## 二、常用的内建函数及实现

&emsp;针对实现，只实现64位无符号整型的版本。其余的版本依此类推。

### **1、__builtin_ctz**

&emsp;一共有三个函数，分别适用于不同的输入类型。

```C++
    int __builtin_ctz (unsigned int x)
    Returns the number of trailing 0-bits in x, starting at the least significant bit position. If x is 0, the result is undefined.

    int __builtin_ctzl (unsigned long)
    Similar to __builtin_ctz, except the argument type is unsigned long.

    int __builtin_ctzll (unsigned long long)
    Similar to __builtin_ctz, except the argument type is unsigned long long.
```
&emsp;这个函数作用是返回输入数二进制表示从最低位开始(右起)的连续的0的个数；如果传入0则行为未定义。三个函数分别用于unsigned int，unsigned long以及unsigned long long。

#### **实现**

```C++
    int __builtin_ctzl(unsigned long x) {
        for (int i = 0; i != 64; ++i)
            if (x >> i & 1) return i;
        return 0;
    }
```

```C++
    int __builtin_ctzl(unsigned long x) {
        int r = 63;
        x &= ~x + 1;
        if (x & 0x00000000FFFFFFFF) r -= 32;
        if (x & 0x0000FFFF0000FFFF) r -= 16;
        if (x & 0x00FF00FF00FF00FF) r -= 8;
        if (x & 0x0F0F0F0F0F0F0F0F) r -= 4;
        if (x & 0x3333333333333333) r -= 2;
        if (x & 0x5555555555555555) r -= 1;
        return r;
    }
```

### **2、__builtin_clz**

&emsp;一共有三个函数，分别适用于不同的输入类型。

```C++
    int __builtin_clz (unsigned int x)
    Returns the number of leading 0-bits in x, starting at the most significant bit position. If x is 0, the result is undefined.

    int __builtin_clzl (unsigned long)
    Similar to __builtin_clz, except the argument type is unsigned long.

    int __builtin_clzll (unsigned long long)
    Similar to __builtin_clz, except the argument type is unsigned long long.
```
&emsp;这个函数作用是返回输入数二进制表示从最高位开始(左起)的连续的0的个数；如果传入0则行为未定义。三个不同的函数分别用于unsigned int，unsigned long以及unsigned long long。

#### **实现**

```C++
    int __builtin_clzl(unsigned long x) {
        for (int i = 0; i != 64; ++i)
            if (x >> 63 - i & 1) 
                return i;
    }
```

```C++
    int __builtin_clzl(unsigned long x) {
        int r = 0;
        if (!(x & 0xFFFFFFFF00000000)) r += 32, x <<= 32;
        if (!(x & 0xFFFF000000000000)) r += 16, x <<= 16;
        if (!(x & 0xFF00000000000000)) r += 8,  x <<= 8;
        if (!(x & 0xF000000000000000)) r += 4,  x <<= 4;
        if (!(x & 0xC000000000000000)) r += 2,  x <<= 2;
        if (!(x & 0x8000000000000000)) r += 1,  x <<= 1;
        return r;
    }
```

### **3、__builtin_ffs**

&emsp;一共有三个函数，分别适用于不同的输入类型。

```C++
    int __builtin_ffs (unsigned int x)
    Returns one plus the index of the least significant 1-bit of x, or if x is zero, returns zero.

    int __builtin_ffsl (unsigned long)
    Similar to __builtin_ffs, except the argument type is unsigned long.

    int __builtin_ffsll (unsigned long long)
    Similar to __builtin_ffs, except the argument type is unsigned long long.
```
&emsp;这个函数作用是返回输入数二进制表示的最低非0位的下标，下标从1开始计数；如果传入0则返回0。三个不同的函数分别用于unsigned int，unsigned long以及unsigned long long。

#### **实现**

&emsp;除输入0以外，满足 __builtin_ffs(x) = __builtin_ctz(x) + 1 。

```C++
    int __builtin_clzl(unsigned long x) {
        if (x == 0) return 0;
        return __builtin_ctz(x) + 1;
    }
```

### **4、__bulitin_popcount**

&emsp;一共有三个函数，分别适用于不同的输入类型。

```C++
    int __builtin_popcount (unsigned int x)
    Returns the number of 1-bits in x.

    int __builtin_popcountl (unsigned long)
    Similar to __builtin_popcount, except the argument type is unsigned long.

    int __builtin_popcountll (unsigned long long)
    Similar to __builtin_popcount, except the argument type is unsigned long long.
```
&emsp;这个函数作用是返回输入的二进制表示中1的个数；如果传入0则返回 0 。三个不同的函数分别用于unsigned int，unsigned long以及unsigned long long。

#### **实现**

```C++
    int __builtin_popcountl(unsigned long x) {
        int r = 0;
        do{
            r += x & 1;
        while (x >>= 1);
        return r;
    }
```

```C++
    int __builtin_popcountl(unsigned long x) {
        int r = 0;
        for (; x; x &= x - 1) ++r;
        return r;
    }
```

```C++
    int __builtin_popcountl(unsigned long x) {
        x = (x & 0x5555555555555555) + (x >> 1  & 0x5555555555555555);
        x = (x & 0x3333333333333333) + (x >> 2  & 0x3333333333333333);
        x = (x & 0x0F0F0F0F0F0F0F0F) + (x >> 4  & 0x0F0F0F0F0F0F0F0F);
        x = (x & 0x00FF00FF00FF00FF) + (x >> 8  & 0x00FF00FF00FF00FF);
        x = (x & 0x0000FFFF0000FFFF) + (x >> 16 & 0x0000FFFF0000FFFF);
        x = (x & 0x00000000FFFFFFFF) + (x >> 32 & 0x00000000FFFFFFFF);
        return x;
    }
```

```C++
    int __builtin_popcountl(unsigned long x) {
        x -= (x >> 1) & 0x5555555555555555;
        x = (x & 0x3333333333333333) + (x >> 2 & 0x3333333333333333);
        x = x + (x >> 4) & 0x0F0F0F0F0F0F0F0F;
        return x * 0x0101010101010101 >> 56;
    }
```
### **5、__bulitin_popcount**

&emsp;一共有三个函数，分别适用于不同的输入类型。

```C++
    int __builtin_parity (unsigned int x)
    Returns the parity of x, i.e. the number of 1-bits in x modulo 2.

    int __builtin_parityl (unsigned long)
    Similar to __builtin_parity, except the argument type is unsigned long.

    int __builtin_parityll (unsigned long long)
    Similar to __builtin_parity, except the argument type is unsigned long long.
```
&emsp;这个函数作用是返回输入的二进制表示中1的个数的奇偶,也就是输入的二进制中1的个数对2取模的结果。三个不同的函数分别用于unsigned int，unsigned long以及unsigned long long。

#### **实现**

&emsp;根据定义，等价于__builtin_popcount(x) & 1 。

```C++
    int __builtin_parityl(unsigned long x) {
        return __builtin_popcountl(x) & 1;
    }
```

```C++
    int __builtin_parityl(unsigned long x) {
        x ^= x >> 1;
        x ^= x >> 2;
        x ^= x >> 4;
        x ^= x >> 8;
        x ^= x >> 16;
        x ^= x >> 32;
        return x & 1;
    }
```

&emsp;这几个函数中都有很巧妙的位运算，大大降低了复杂度。关于位运算的更多例子，可以参考链接[ \[4\] ](http://graphics.stanford.edu/~seander/bithacks.html)。

[1]https://gcc.gnu.org/onlinedocs/gcc-4.3.2/gcc/Other-Builtins.html#Other-Builtins
[2]https://www.cnblogs.com/justinh/p/7754655.html  
[3]https://xr1s.me/2018/08/23/gcc-builtin-implementation/
[4]http://graphics.stanford.edu/~seander/bithacks.html
