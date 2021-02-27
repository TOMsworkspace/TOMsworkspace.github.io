---
title: C++关键字与保留标识符
top_img: 'https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2020/11/25/C++关键字与保留标识符/figure1.jpg'
comments: true
cover: 'https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2020/11/25/C++关键字与保留标识符/figure1.jpg'
toc: true
toc_number: true
copyright: true
date: 2020-11-25 15:15:19
updated:
tags: C 关键字 保留标识符
categories: C/C++
keywords: C 关键字 保留标识符
description: C++的关键字与保留标识符简介和总结
---

# C++语言关键字和保留标识符

## 关键字

&emsp;关键字是组成编程语言词汇表的标识符，不能将他们用于其他用途。下表列出了C++所有关键字及，包括C++11,14,17及20标准对于关键字用途的重新定义。

|关键字  |标准  |描述|
|:--:   |:--:  |:--|
|alignas|C++11  |用于内存对齐相关|
|alignof|C++11  |用于内存对齐相关| 
|asm    |C++11  |用于在C++代码中直接插入汇编语言代码|
|auto   |C++98,C++11|C++ 98 中，auto 的作用是让变量成为自动变量（拥有自动的生命周期），但是该作用是多余的，变量默认拥有自动的生命周期。在C++11 中，已经删除了该用法，取而代之的作用是：自动推断变量的类型。|
|bool   |C++11|声明布尔类型变量|
|break  |C++98|跳出循环语句|
|case   |C++98|用于switch分支语句|
|catch  |C++11|异常处理，与try一起用于捕获并处理异常|
|char   |C++98|声明字符类型|
|char16\_t|C++11|声明UTF-16字符集表示的字符类型，要求大到足以表示任何 UTF-16 编码单元（16位）。|
|char32\_t|C++11|声明UTF-32字符集表示的字符类型，要求大到足以表示任何 UTF-16 编码单元（32位）。|
|class|C++98,C++11|1）声明类;声明有作用域枚举类型(C++11 起);2）在模板声明中，class 可用于引入类型模板形参与模板模板形参;3）若作用域中存在具有与某个类类型的名字相同的名字的函数或变量，则 class 可附于类名之前以消歧义，这时被用作类型说明符|
|const  |C++98   |可出现于任何类型说明符中，以指定被声明对象或被命名类型的常量性（constness）。|
|const\_cast|C++11|在有不同 cv 限定(const and volatile)的类型间进行类型转换。|
|constexpr|C++11,14,17|constexpr 说明符声明可以在编译时求得函数或变量的值。然后这些变量和函数（若给定了合适的函数实参）可用于编译时生成常量表达式。用于对象或非静态成员函数 (C++14 前)声明的constexpr说明符蕴含const。用于函数声明的 constexpr说明符或static 成员变量 (C++17 起)蕴含inline。若函数或函数模板的任何声明拥有constexpr说明符，则每个声明必须都含有该说明符。|
|continue|C++98|跳出当前循环，开始下一次循环|
|dealtype|C++11,14,17|检查实体的声明类型，或表达式的类型和值类别。对于变量，指定要从其初始化器自动推导出其类型。对于函数，指定要从其return语句推导出其返回类型。(C++14 起)对于非类型模板形参，指定要从实参推导出其类型。(C++17 起)|
|default|C++98|用于switch分支语句|
|delete|C++11|解内存分配运算符，与new一起管理动态分配内存；弃置函数，如果取代函数体而使用特殊语法=delete，则该函数被定义为弃置的（deleted）。|
|do     |C++98|do-while循环语句|
|double |C++98|声明双精度浮点数类型|
|dynastic\_cast|C++11|类型转换运算符，沿继承层级向上、向下及侧向，安全地转换到其他类的指针和引用。|
|else   |C++98|if-else条件语句|
|enum   |C++98 |声明枚举类型|
|explicit|C++11,17,20|1) 指定构造函数或转换函数(C++11 起)或推导指引(C++17起)为显式，即它不能用于隐式转换和复制初始化。2) explicit说明符可以与常量表达式一同使用。当且仅当该常量表达式求值为true时函数为显式(C++20 起)。explicit说明符只可出现于在类定义之内的构造函数或转换函数(C++11 起)的声明说明符序列中。|
|export|C++98,11,20|用于引用文件外模板声明(C++11 前)。不使用并保留该关键词(C++11 起)(C++20 前)。标记一个声明、一组声明或另一模块为当前模块所导出(C++20 起)。|
|extern |C++98|具有外部链接的静态存储期说明符,显式模板实例化声明|
|false|C++11|布尔值假|
|float  |C++98|声明单精度的浮点类型|
|for    |C++98|for循环|
|friend|C++11|友元声明出现于类体内，并向一个函数或另一个类授予对包含友元声明的类的私有及受保护成员的访问权。|
|goto   |C++98|程序跳转到指定的位置|
|if     |C++98|if条件语句|
|inline |C++98|声明内联类型|
|int    |C++98|声明整形类型|
|long   |C++98|声明长整型|
|mutable|C++11|可出现于任何类型说明符（包括声明文法的声明说明符序列）中，以指定被声明对象或被命名类型的常量性（constness）或易变性（volatility）。|
|namespace|C++11|声明名称空间以避免名称冲突|
|new|C++11|分配运算符，与delete一起管理动态分配内存。|
|noexcept|C++11|1）noexcept运算符,进行编译时检查，若表达式声明为不抛出任何异常则返回true。2）noexcept说明符，指定函数是否抛出异常。|
|nullptr|C++11|指针字面量，用于表示空指针|
|operator|C++11|为用户定义类型的操作数重载C++运算符。|
|private|C++11|访问说明符。在class/struct或union的成员说明中，定义其后继成员的可访问性。|
|protected|C++11|访问说明符。在class/struct或union的成员说明中，定义其后继成员的可访问性。|
|public|C++11|访问说明符。在class/struct或union的成员说明中，定义其后继成员的可访问性。|
|register|C++98,17|自动存储期说明符(弃用)。(C++17 前)register关键字请求编译器尽可能的将变量存在CPU内部寄存器中，而不是通过内存寻址访问，以提高效率。不使用并保留该关键词(C++17 起)。|
|reinterpret\_cast|C++11|类型转换运算符。通过重新解释底层位模式在类型间转换。|
|return |C++98|函数返回|
|short  |C++98|声明短整型数据类型|
|signed |C++98|声明带符号的数据类型|
|sizeof |C++98|返回指向的数据对象或类型所占空间的大小，以字节(byte)为单位|
|static|C++98|声明具有静态存储期和内部链接的命名空间成员,定义具有静态存储期且仅初始化一次的块作用域变量,声明不绑定到特定实例的类成员|
|static\_assert|C++11|声明编译时检查的断言|
|static\_cast|C++11|类型转换运算符。用隐式和用户定义转换的组合在类型间转换。|
|struct|C++98|声明结构体变量类型|
|switch |C++98|switch分支语句|
|template|C++11|声明模板类型|
|this|C++11|关键字this是一个纯右值表达式，其值是隐式对象形参（在其上调用非静态成员函数的对象）的地址。它能出现于下列语境：1) 在任何非静态成员函数体内，包括成员初始化器列表；2) 在非静态成员函数的声明中，（可选的）cv 限定符（const and volatile）序列之后的任何位置，包括动态异常说明(弃用)、noexcept 说明(C++11)以及尾随返回类型(C++11 起)3) 在默认成员初始化器中。(C++11 起)|
|thread\_local|C++11|声明属于创建线程私有的线程局部数据变量|
|throw|C++11|抛出异常|
|true|C++11|布尔值真|
|try|C++11|异常处理，与catch一起用于捕获并处理异常|
|typedef|C++98|创建能在任何位置替代（可能复杂的）类型名的别名。|
|typeid|C++11|查询类型的信息。用于必须知晓多态对象的动态类型的场合以及静态类型鉴别。|
|typename|C++11,17|在模板声明中，typename可用作class的代替品，以声明类型模板形参和模板形参(C++17 起)。在模板的声明或定义内，typename可用于声明某个待决的有限定名类型。|
|union|C++98|声明联合体类型变量|
|unsigned|C++98|声明无符号类型变量|
|using|C++11|对命名空间的using指令及对命名空间成员的using声明;对类成员的using声明;类型别名与别名模板声明。(C++11 起)|
|virtual|C++11|说明符指定非静态成员函数为虚函数并实现运行时多态。用于声明虚基类。|
|void|C++98|声明无(void)类型的变量,无形参函数的形参列表。|
|volatile|C++98|可出现于任何类型说明符中，以指定被声明对象或被命名类型的易变性（volatility）。|
|wchar\_t|C++11|宽字符类型。要求大到足以表示任何受支持的字符编码。|
|while|C++98|do-while循环语句|

&emsp;将所有关键字分一下类大概可以分为如下几类

- 数据类型，包括auto,bool,char,char16\_t,char32\_t,class,double,enum,false,float,int,long,nullptr,short,signed,struct,template,true,union,unsigned,void,wchar\_t。  
- 存储类别说明符，包括auto,extern,register,static,thread\_local,
- 程序控制，包括asm,break,case,continue,default,do,else,for,goto,if,rturn,switch,while。
- 类型限定，包括const,mutable,volatile。
- 类型转换，包括const\_cast,dynastic\_cast,reinterpret\_cast,static\_cast,explicit。
- 异常处理，包括catch,try,throw,noexcept。
- 内存管理相关，包括new,delete,alignas,alignof,sizeof。
- 类相关，包括class,explicit,friend,namespace,operator,private,protected,public,template,this,using,virtual。
- 编译优化相关，包括constexpr,inline,static\_assert。
- 其他，包括export,dealtype,typedef,typeid,typename。。

## 替代标记

&emsp;除关键字外，C++有一些运算符能被字母替代，成为替代标记。运算符的替代标记如下表：

|    标记   |  含义  |     标记   |  含义|  
|:--------:|:------|:---------:|:-------|
|and|&&|and\_eq|&=|
|bitand|&|bitor|\||
|compl|~|not|!|
|not\_eq|!=|or|\|\||
|or\_eq|\|=|xor|^|
|xor\_eq|^=|



## 保留标识符

&emsp;还有一些保留标识符。C++语言已经指定了它们的用途或保留它们的使用权，如果你使用这些标识符,后果是不确定的。也就是说，可能导致编译错误、警告、程序不能正确运行或者根本不会导致任何问题。

&emsp;C++语言保留了库头文件中使用的宏名。如果程序中包含了某个头文件，则不应该将该头文件中定义的宏名用作其他目的。C++语言保留了以两个下划线或下划线和大写字母打头的名称，还将以单个下划线打头的名称保留用作全局变量。C++语言保留了在库头文件中被声明为外部链接性的名称。如函数特征标（名称和参数列表）。

## 有特殊含义的标识符

&emsp;C++使用具有特殊意义的标识符来避免新增关键字。这些标识符不是关键字，但用于实现语言功能。编译器根据上下文来判断他们是常规标识符还是用于实现语言功能。例如，virtual,delete,final,override等。