---
title: C++中的指针与引用
top_img: 'https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/03/03/C++中的指针与引用/figure1.jpg'
comments: true
cover: 'https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/03/03/C++中的指针与引用/figure1.jpg'
toc: true
toc_number: true
copyright: true
date: 2021-03-03 20:47:52
updated:
tags: 
    - 指针
    - 引用
categories: C/C++
keywords: 指针、引用
description: C++中的指针与引用的联系与区别
---
&emsp;C/C++中的指针是一个既让人喜欢又恨的特性。使用指针，既方便了代码的编写，但是随之而来的就是各种因空指针、野指针导致的问题。随着语言的发展，C++中出现了引用这个C语言中所没有的特性。因为引用与指针的 “貌合神离” 使得许多人对指针与引用的正确使用一头雾水，摸不着头脑。

## 一、指针与引用
### 1、**指针**
&emsp;指针，是一个特殊的变量，与int、float之类的变量一样存储变量的值。只是这个值比较特殊：它是另一个变量的地址。对于一个不受限定的指针（非const，非volatile），C++支持通过对指针变量的操作来间接访问和修改它存储的地址的值。因为他是一个变量，所以它存在地址空间，也可以通过修改它存储的地址值来让它指向另一个对象。

&emsp;对于一个不受限的指针，由于允许它不存储任何变量的地址（空指针）。而且支持多级指针。复杂的语义使得往往在使用时出现各种问题，尤其是在指针作为函数的参数和返回值的时候。出于对于安全和效率的考虑，C++出现了引用这个语言特性。

### 2、**引用**
&emsp;简单来说，引用是已定义变量的别名。就如同一个人有几个名字一样，一个大名几个外号一样，无论叫哪个都是在叫。对于别名的读取修改等全部操作也就是对于原变量的操作。就像typedef关键字给类型取别名一样，引用就是给变量取别名。给变量取别名有几条规则：
- （1）引用被创建的同时必须被初始化。
- （2）不能有NULL引用，引用必须与合法的存储单元关联。
- （3）一旦引用被初始化，就不能改变引用的关系。 

&emsp;(1)(2)条规则说的是一个东西。给人取外号你得首先要有这么人吧，不能先取一个外号逮着谁谁就叫这个名。(3)的意思是一旦给人取了外号，这外号就跟着你了，直到人没了（变量销毁）。一个外号不能今天是张三，明天是李四吧。那我找这个外号是找张三还是找李四啊。如果没有这条规则的限定就可能出现很复杂的情况了。
## 二、指针与引用的区别

从引用和指针的定义和规则来看:

1、指针的值可以为空，但是引用的值不能为NULL，"**并且引用在定义的时候必须初始化**"。

&emsp;由于指针在定义时并不要求必须赋初值，所以指针可能是空(NULL)值；但是引用规则要求它必须在定义时初始化，所以它不能为空值，即不能绑定到空的对象。**这使得指针在使用前必须对其进行非空检查，而引用就不需要了**。在多重函数调用和返回时往往容易忘记对指针的非空判断而导致程序异常；引用则不需要对其进行检查。引用这个特性最初被引入的主要原因就是用于作为函数的形参。

&emsp;"引用在定义时必须初始化"这句话真的没问题么？我觉得是不太严谨的。看下面的例子：

```C++
struct  Test{
    int a[5];
    int &b;
};

cout << sizeof(tag) << endl;
```
&emsp;这段代码是可以编译通过的，在64位下x86机器下输出结果是32。这个结构体定义了一个引用，但是并未初始化。下面对结构体实例一下:
```C++
struct  Test{
    int a[5];
    int &b;
};

Test test;

cout << sizeof(tag) << endl;
```
&emsp;这段代码是无法编译通过的，错误是没有合适的构造函数可用。因此“定义时必须初始化”这句话应该改成“分配空间时必须初始化”更合适一些。

2、**引用只能在定义时被初始化一次，之后不可变；指针在初始后依然可变。**

&emsp;引用初始化后就不可变的作用类似于一个指针常量，即一个指向了对象之后就不能更改使其指向其他的对象的指针。我们都知道，语言特性只是定义于语言编译器的一些特殊规则。  
&emsp;实际上，多数编译器对引用的底层实现就是一个指针常量加上一些语法规则。引用与指针常量的区别就在于指针常量定义时可以不需要初始化，而且指针与引用访问绑定对象的语法规则也不一样，不过这都只是一些编译器层面的区别。

3、指针是一个实体，需要分配内存空间。引用只是变量的别名，"**不需要分配内存空间**"。

&emsp;引用把对本身的一切操作都对应到其绑定的对象上，包括取地址运算。所以对引用变量取地址得到的是它绑定对象的地址。实际上，编译器对我们隐藏了引用变量的地址即空间的信息，当对引用变量取地址时返回的是其绑定变量的地址。从这个现象看来，引用变量似乎是不占地址空间的，但是实际上真的是这样么？恐怕并不是。

&emsp;之前那个例子的输出是32，5个int占20bytes，由于内存对齐，数组要占24个字节，那剩下的8个bytes的空间应该是引用变量所用。所以引用不需要分配空间的说法并不严谨。

&emsp;再考虑这样一个场景：当一个接受引用参数的函数在实际运行时，传入的变量绑定到一个引用变量上，无论传入的是一个变量还是一个同类型的变量引用，实际上传入的都是变量的地址，那在函数运行的过程中，肯定是需要空间来保存这个地址的。而且在函数返回引用时也是一样。

&emsp;那是不是引用变量就一定需要空间呢？我觉得也不一定。看下面一段代码：

```C++
    int a =1;
    int & b = a;

    cout << a << " " << b << endl;

```

&emsp;这里就不需要也没必要再来浪费空间来保存一个引用变量，编译器在编译时就可以直接使用原变量来直接替换这个引用。[实际上，标准并没有规定引用要不要占用内存，也没有规定引用具体要怎么实现，具体随编译器。](http://bbs.csdn.net/topics/320095541)不过，大多数编译器实现是占用了空间的。 


4、**有多级指针，但是没有多级引用，只能有一级引用。**

&emsp;根据引用的定义，是没有也不需要多级引用的存在的。想一想，你有一个外号了，还要给你的外号取外号有用么，没有必要吧？但是指针的指针就有其用处了，多级指针是有用处的，如定义多维数组。

&emsp;注意，在C++11中，出现了例如 int && a 的用法，这是C++11引入的新特性 “**右值引用**” 而不是多级引用。

5、**指针和引用的自增运算结果不一样。**

&emsp;指针自增的结果是指向下一个地址，与原本的地址相差一个指针类型的空间大小；引用加一时引用的绑定的变量值加1。

6、sizeof引用得到的是所指向的变量（对象）的大小，而sizeof 指针得到的是指针本身的大小。

7、**没有const引用，有const指针。**

&emsp;例如对于int 类型，具体指没有int& const a这种形式，而const int& a是有 的，前者指引用本身即别名不可以改变，这是当然的（引用规则规定），所以没有也不需要这种形式，后者（常量引用或常引用）指引用所指的值不可以改变，这种用法是存在的。

&emsp;这与指针类似，当const修饰指针类型时。根据关键字出现的地方有指针常量和常量指针以及指向常量的常量指针三种用法。指针常量表示一个指向常量的指针，不可用指针修改指向变量的值，但是可修改指针的值使其指向其它变量；指针常量表示不能修改指针使其指向其它变量，但是可以修改指向变量的值，这与引用相对应；指向常量的指针常量表示不可以修改指针指向的变量也不能修改指针使其指向其它变量，这与常量引用相对应。比较绕，请看下面的代码：

```C++
    //指针
    //1.常量指针
    const int * p;
    int const * p;

    //2.指针常量
    int * const p;

    //3.指向常量的指针常量
    int const * const p;
    const int * const p;

    //引用
    //1.引用,类似于指针常量
    int & a; //合法
    int & const a; //不存在也不需要

    //2.常量引用，类似于指向常量的指针常量
    const int & a;
    int const & a;
```

8、**引用比指针更加安全**

&emsp;这个区别也是来自于引用的规则定义导致的。引用在定义时就与变量绑定了，而指针不一定，指针在定义后没有初始化就是空指针。引用与被引用的变量是同一个地址，编译器隐藏了引用的地址不能对引用本身进行地址操作，这样使地址是不可修改的，使访问更加安全。指针可能导致空指针、野指针、数组越界之类的问题，但引用不会。

## 三、为什么要有引用

&emsp;不存在指向空值的引用这个事实，意味着使用引用的代码效率比使用指针的要高。因为在使用引用之前不需要测试它的合法性。相反，指针则应该总是被测试，防止其为空。

&emsp;而指针可能导致空指针、野指针、数组越界之类的问题，但引用不会。但是要注意，在返回引用的函数中，应避免返回局部变量的引用这种用法。局部变量在函数退出时被销毁，返回其引用导致引用绑定到不存在的变量上，这会导致不可预见的错误。



[1]https://www.runoob.com/w3cnote/cpp-difference-between-pointers-and-references.html
[2]https://blog.csdn.net/zhengqijun_/article/details/54980769?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control&dist_request_id=1328592.13329.16147746419368463&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control
[3]https://blog.csdn.net/qq_27678917/article/details/70224813
[4]https://blog.csdn.net/superwangxinrui/article/details/80565594?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_baidulandingword-2&spm=1001.2101.3001.4242
[5]https://blog.csdn.net/weixin_30270889/article/details/97389623?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.control&dist_request_id=1328593.13456.16147750581565135&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.control
[6]https://blog.csdn.net/qq_39539470/article/details/81273179?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control&dist_request_id=1328593.13456.16147750581565135&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control
[7]https://blog.csdn.net/wanttifa/article/details/100904422
[8]https://blog.csdn.net/gmcxsqjh/article/details/3719552?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control&dist_request_id=&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control
[9]https://blog.csdn.net/vczlyz/article/details/79214334?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control&dist_request_id=1328592.13326.16147803071270639&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control
[10]https://blog.csdn.net/dimengban0741/article/details/101880410?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control&dist_request_id=&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control
[11]https://bbs.csdn.net/topics/320095541

