---
title: C++中const变量的修改与赋值
top_img: 'https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/02/25/C++中const变量的修改与赋值/figure1.jpg'
comments: true
cover: 'https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/02/25/C++中const变量的修改与赋值/figure1.jpg'
toc: true
toc_number: true
copyright: true
date: 2021-02-25 20:26:16
updated:
tags: 
    - C++
    - Const
categories: C/C++
keywords: 
    - C++
    - const
description: C++中const变量的修改与赋值
---

&emsp;出于避免对数据的无意修改的需求，C及C++语言引入了const关键字。与C语言中的const相比，C++中的const具有更丰富的用法。C++中的const除了可用于修饰变量，指针，函数及函数参数，还可用于修饰类对象，类成员，类成员函数等。由于其丰富的用途，往往容易对其产生误解，尤其是与指针用法结合在一起时。下面以一些实例来说明这个问题。

## 一、const修饰变量

### 1、含义
&emsp;当使用const修饰普通变量时。如
```C++
    int const age = 39;
    //或
    const int age = 39;
```
&emsp;表示const关键字修饰的这个变量是一个常量，不能对其值进行修改。但实际上这句话不是绝对的。根据C++标准，对于修改const变量，属于未定义行为（指行为不可预测的计算机代码），这样一来这个行为的结果取决于各种编译器的具体实现（即不同编译器可能表现不同）。

### 2、修改const变量
&emsp;下面来说一下如何修改const修饰的变量。可以使用直接赋值和使用指针来修改其值。如：

```C++
    const int age = 39;

    //1.直接赋值
    age = 40;

    //2.使用指针修改
    int * p = (int *)&age;
    *p = 40;

    cout << "age=" << age << endl;
    cout << "*p=" << *p << endl;
    cout << "&age=" << &age << endl;
    cout << "p=" << (void*)p << endl;
    cout << "&p=" << &p << endl;
```
&emsp;这段代码对于age变量是全局变量还是局部变量结果并不一样。
#### （1）修改局部变量
&emsp;程序能正常运行，且常量被修改了，但是有一个问题：输出age的值和\*p的值并不相同，age的值还是39，而*p的值是40。而且p确实指向age所在的地址空间。

&emsp;这是什么原因呢？难道一个地址空间可以存储不同的俩个值？当然不能。这就是C++中的**常量折叠**：const变量（即常量）值放在编译器的符号表中，计算时编译器直接从表中取值，省去了访问内存的时间，这是编译器进行的优化。age是const变量，编译器对age在预处理的时候就进行了替换。编译器只对const变量的值读取一次。所以打印的是39。age实际存储的值被指针p所改变。但是为什么能改变呢，从其存储地址可以看出来，其存储在堆栈中。
#### （2）修改全局变量
&emsp;程序编译通过，但运行时错误。编译器提示age存储的空间不可写，也就是没有写权限，不能修改其值。    
&emsp;原因是age是全局变量，全局变量存储在全局空间，且只有可读属性，无法修改其值。
#### （3）volatile关键字

&emsp;由于编译器对const修饰的局部变量进行了优化，使得对于const变量值的读取实际没有进入内存。而在此基础上加上volatile关键字，即告诉编译器该变量属于易变的，不要对此句进行优化，每次计算时要去内存中读取该变量的值,进而避免出现**常量折叠**的问题。可以这样做：
```C++
    const volatile int age = 39;

    //1.直接赋值
    age = 40;

    //2.使用指针修改
    int * p = (int *)&age;
    *p = 40;

    cout << "age=" << age << endl;
    cout << "*p=" << *p << endl;
    cout << "&age=" << &age << endl;
    cout << "p=" << (void*)p << endl;
    cout << "&p=" << &p << endl;
```

&emsp;这里也有个问题：每种编译器对volatile关键字的支持效果并不一致，有的编译器就直接“不理会”，如VC++6.0编译器[1]，使得依然无法避免常量折叠的问题。
#### （4）强制类型转换
&emsp;const修饰的变量在C++中被看作是常量，无法修改。但是存在这样的场景：**有时候我们需要一个一个值，它在大多数时候是常量，但是在又是又是可以更改的。**这时候就需要使用指针来对const变量进行修改。但是在上面的例子中使用到了强制类型转换：
```C++
    const volatile int age = 39;
    //使用指针修改

    //不使用强制类型转换
    int * p = &age;  

    //强制类型转换
    int * p = (int *)&age;  
    //或
    int * p = const_cast<int *>(&age); 

    *p = 40;
```
&emsp;在这里如果不使用强制类型转换，等于是将一个const int * 类型赋值给一个int * 类型，无法编译通过。必须使用强制类型转换。
## 二、const 修饰指针

&emsp;当使用const修饰指针变量时，情况更加复杂起来了。const可以放置在不同的地方，因此具有不同的含义。请看下面一个例子：

```C++
    int age = 39;
    
    const int * p = &age;
    
    int const * p = &age;

    int * const p = &age;

    int * p const = &age;
```
&emsp;在这个例子中，一二两种情况是一个意思，表示p是一个指向const的指针；第三种用法表示p是一个常量指针；第四种情况不符合语法。
### 1、指向常量（const）的指针和常量(const)指针

&emsp;故名思意：**指向常量的指针**表示const修饰的指针是指向常量的，不能用这个指针来修改它指向的值，但是可以让该指针指向其他地址。这里就有个问题：age明明是一个变量，为什么可以赋值给一个指向常量(const)的指针？原因是这里发生了隐式的类型转换，&age是一个int * 类型，而p是一个const int * 类型，从int *到const int *是从大范围到小范围，C++进行了隐式的转换，所以能够赋值成功。前一节的从const int * 到int * 是从小范围到一个大范围，无法进行隐式类型转换，所以必须进行强制（显式）类型转换才能赋值成功就是这个原因了。

&emsp;**常量指针**表示指针的值是常量，也就是不能修改指针指向的位置，但是可以修改该指针指向的变量的值。

&emsp;那一个指针能不能既是指向常量的指针又是常量指针呢？这是可以的。例如：
```C++
    int age = 39;
    
    const int * const p = &age;
```
&emsp;此时的p表示既不能修改它指向的位置的变量的值，又不能使p指向其他位置的变量。
### 2、修改指向常量的指针
&emsp;使用如下的例子来修改指向常量的指针：
```C++
    int age = 39;
    int year = 2021;
    
    const int * p = &age;

    //修改p指向变量的值,编译错误
    *p = 40;
    //修改指针p的值，可以修改
    p = &year;
```
&emsp;运行发现确实无法修改p指向的值，编译错误；但是可以修改指针P的值，让它指向另外的变量year。
### 3、修改常量指针
&emsp;使用如下的例子来修改常量指针：
```C++
    int age = 39;
    int year = 2021;

    int * const p = &age;

    //修改p指向变量的值,可以修改
    *p = 40;
    //修改指针p的值，编译错误
    p = &year;
```
&emsp;运行发现可以修改p指向的值；但是不能修改指针P的值，让它指向另外的变量year。

&emsp;对于既是既是指向常量的指针又是常量指针，则既无法修改指向的值，又无法修改指针的值，让它指向另外的变量。
```C++
    int age = 39;
    int year = 2021;

    const int * const p = &age;

    //修改p指向变量的值,编译错误
    *p = 40;
    //修改指针p的值，编译错误
    p = &year;
```
## 三、使用const的好处

### 1、可以定义const常量
&emsp;这样可以避免由于无意间修改数据而导致的编程错误。
### 2、便于进行类型检查
&emsp;const常量有数据类型，而宏常量没有数据类型。编译器可以对前者进行类型安全检查，而对后者只进行字符替换，没有类型安全检查，并且在字符替换时可能会产生意料不到的错误。
### 3、为函数重载提供了一个参考
&emsp;const修饰的函数可以看作是对同名函数的重载。
### 4、可以节省空间，避免不必要的内存分配
&emsp;const定义常量从汇编的角度来看，只是给出了对应的内存地址，而不是象宏一样给出的是立即数，所以，const定义的常量在程序运行过程中只有一份拷贝，而宏定义的常量在内存中有若干个拷贝。
### 5、提高了效率
&emsp;编译器通常不为普通const常量分配存储空间，而是将它们保存在符号表中，这使得它成为一个编译期的常量，没有了存储与读内存的操作，使得它的效率也很高。

[1]https://blog.csdn.net/zz460833359/article/details/48917217
[2]https://blog.csdn.net/niusi1288/article/details/90710176?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_title-3&spm=1001.2101.3001.4242  
[3]https://blog.csdn.net/qq_30968657/article/details/53837022       
[4]https://blog.csdn.net/Eric_Jo/article/details/4138548?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control&dist_request_id=1ba4e5e7-1d6a-4d31-9b36-5fcc5bb702fa&depth_1-utm_source=distribute.pc_relevant.none-task-blog-Blog CommendFromMachineLearnPai2-1.control  
[5]https://blog.csdn.net/Eric_Jo/article/details/4138548?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control&dist_request_id=1ba4e5e7-1d6a-4d31-9b36-5fcc5bb702fa&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control
 