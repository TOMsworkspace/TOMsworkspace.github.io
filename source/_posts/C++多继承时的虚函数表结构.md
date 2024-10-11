---
title: C++多继承时的虚函数表结构
top_img: 'https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2020/09/26/C++多继承时的虚函数表结构/figure1.jpg'
comments: true
cover: 'https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2020/09/26/C++多继承时的虚函数表结构/figure1.jpg'
toc: true
toc_number: true
copyright: true
date: 2020-09-26 17:40:36
updated:
tags: 
            - C++
            - 多继承
            - 虚函数表结构
categories: C/C++
keywords:
            - C++
            - 多继承
            - 虚函数表
description: 一个多继承时虚函数表的结构的例子
---

&emsp;C++为了实现运行时的多态，引入了虚函数的概念。为了实现运行时多态的，其底层一般采用虚函数表来实现对虚函数的动态绑定，进而在基类对象的引用或指针在调用同名的虚函数时可以根据引用或指针指向对象的实际类型调用相应的函数。当类的继承关系中没有使用多继承时，对象的虚函数表结构还相对简单；然而继承中出现多集成时，问题就变得复杂起来了。

&emsp;有如下的一个例子：

```c++
class A
{
public:

    A(int a=0):a(a){}

    virtual void fun1()
    {
        cout << "A::fun1()" <<endl;
    }

private:

    int a;
};

class Base1: public A
{
public :
    Base1(int b1=1):A(0),b1(b1) {}

    virtual void func1()
    {
        cout<<"Base1::func1" <<endl;
    }
    virtual void func2()
    {
        cout<<"Base1::func2" <<endl;
    }

private:

    int b1;
};
class Base2:  public A
{
public :
    Base2(int b2=2):A(0),b2(b2) {}

    virtual void func1()
    {
        cout<<"Base2::func1" <<endl;
    }
    virtual void func2()
    {
        cout<<"Base2::func2" <<endl;
    }

private:

    int b2;
};
class Derive :  public Base2,  public Base1
{
public :
    Derive(int c3=3):Base1(1),Base2(2),c3(c3) {}

    virtual void func1()
    {
        cout<<"Derive::func1" <<endl;
    }
    virtual void func3()
    {
        cout<<"Derive::func3" <<endl;
    }

private:

    int c3;
};

```

&emsp;在这个例子中的A、Base1、Base2的虚函数表遵循一般情况下的定义，在每个类的地址空间的头8B(64位)或4B(32位)的空间保存了一个指向该类虚函数表的指针。通过这个指针可以访问其虚函数表。虚函数表的每一个表项保存了指向该类的所有虚函数的函数指针。

&emsp;理论上是这样，我们把它输出来看。

```c++
typedef void (* FUNC) ();
typedef unsigned long long ull;

//输出虚函数表
void PrintVTable (ull* VTable)
{
    cout<<" 虚表地址>"<< VTable<<endl ;

    int i;
    for ( i = 0; (VTable[i])!=NULL ; ++i)
    {
        cout<<i<<ends ;
        printf(" 第%d个虚函数地址 :0X%x,->", i , VTable[i]);
        FUNC f = (FUNC)VTable[i];
        f();
    }
    cout<<endl ;

}

//输出内存内容
void PrintMem(A & obj){
    int a = sizeof(obj)/8;

    for (int i = 0; i < a; ++i) {
        ull temp =*((ull*)(&obj)+i) ;
        cout << "整体:" << temp << " 高4B:" << (temp>>32) << " 低4B:" << (unsigned int)temp << endl;
    }

}

int main()
{
    cout << "sizeof A:" << sizeof(A) << endl;
    cout << "sizeof Base1:" << sizeof(Base1) << endl;
    cout << "sizeof Base2:" << sizeof(Base2) << endl;
    cout << "sizeof Derive:" << sizeof(Derive) << endl;

    A a(1);

    ull* VTable = (ull*)(*(ull*)(&a));
    PrintMem(a);
    PrintVTable(VTable);

    Base1 b1(2);
    VTable =(ull *) (*(ull*)(&b1));
    PrintMem(b1);
    PrintVTable(VTable);

    return 0;
}
```

```bash
sizeof A:16
sizeof Base1:16
sizeof Base2:16
sizeof Derive:40
整体:93869941914944 高4B:21855 低4B:3431660864
整体:93866510254081 高4B:21855 低4B:1
虚表地址>0x555fcc8afd40
第1个虚函数地址 :0Xcc6aef42,->A::fun1()
```

&emsp;对于A，占有16B内存，虚函数表指针占8B，数据成员a实际只占4B，由于内存对齐，所以sizeof A是16 B。对于Base1或Base2.输出结果如下:

```bash
sizeof Base1:16
整体:94892606594304 高4B:22093 低4B:3894123776
整体:8589934593 高4B:2 低4B:1
虚表地址>0x564de81b9d00
第1个虚函数地址 :0Xe7fb9032,->A::fun1()
第2个虚函数地址 :0Xe7fb90a6,->Base1::func1
第3个虚函数地址 :0Xe7fb90de,->Base1::func2
```

&emsp;为什么Base1的长度也是16 B呢？是因为来自A的a和b1 成员分别使用了高4B和低4 B。

&emsp;这个例子中还出现了菱形继承的情况：Base1和Base2继承自A，而Derive又同时继承自Base1和Base2。同时每个类中都带有虚函数。那这时候Derive是怎么处理自有的虚函数和继承来的虚函数的呢？它的虚函数表又是什么样的结构呢?先输出其内存内容。

```c++

    cout << "sizeof Derive:" << sizeof(Derive) << endl;
    Derive d(4);

    ull * VTable =(ull *) (*(ull*)(&d));

    PrintMem(d);
```

```bash
sizeof Derive:40
整体:94560403680304 高4B:22016 低4B:2403691568
整体:12884901889 高4B:3 低4B:1
整体:94560403680352 高4B:22016 低4B:2403691616
整体:8589934593 高4B:2 低4B:1
整体:140733193388036 高4B:32767 低4B:4
```

&emsp;可以看到， Derive占40B内存，第一个8B是一个虚函数表地址，第二个8B分别是A的a成员和Base2的b2成员，第三个8B又是一个虚函数表地址，第四个8B是又一个A的a成员和Base1的b1成员。最后一个8B，低4B是Derive的c3成员，高四B未使用。

接下来去输出其两个虚函数表：

```bash
虚表地址>0x564544793c30
第1个虚函数地址 :0X44593286,->A::fun1()
第2个虚函数地址 :0X44593476,->Derive::func1
第3个虚函数地址 :0X445933de,->Base2::func2
第4个虚函数地址 :0X445934b4,->Derive::func3

虚表地址>0x564544793c60
第1个虚函数地址 :0X44593286,->A::fun1()
第2个虚函数地址 :0X445934ad,->Derive::func1
第3个虚函数地址 :0X44593332,->Base1::func2
```

&emsp;这是还没有使用虚继承的情况，当使用虚继承是又会是什么样呢？

```c++
class A
{
public:

    A(int a=0):a(a){}

    virtual void fun1()
    {
        cout << "A::fun1()" <<endl;
    }

private:

    int a;
};

class Base1: virtual public A
{
public :
    Base1(int b1=1):A(0),b1(b1) {}

    virtual void func1()
    {
        cout<<"Base1::func1" <<endl;
    }
    virtual void func2()
    {
        cout<<"Base1::func2" <<endl;
    }

private:

    int b1;
};
class Base2: virtual public A
{
public :
    Base2(int b2=2):A(0),b2(b2) {}

    virtual void func1()
    {
        cout<<"Base2::func1" <<endl;
    }
    virtual void func2()
    {
        cout<<"Base2::func2" <<endl;
    }

private:

    int b2;
};
class Derive :  public Base2,  public Base1
{
public :
    Derive(int c3=3):Base1(1),Base2(2),c3(c3) {}

    virtual void func1()
    {
        cout<<"Derive::func1" <<endl;
    }
    virtual void func3()
    {
        cout<<"Derive::func3" <<endl;
    }

private:

    int c3;
};
```

这时输出Derive对象的内存及虚函数表:

```bash

sizeof Derive:48
整体:94345236515712 高4B:21966 低4B:1984891776
整体:94343251623939 高4B:21966 低4B:3
整体:94345236515760 高4B:21966 低4B:1984891824
整体:17179869186 高4B:4 低4B:2
整体:94345236515800 高4B:21966 低4B:1984891864
整体:94343251623937 高4B:21966 低4B:1

```

&emsp;可以看到此时Derive占48B，第一个8B是一个虚函数表指针，第二个8B的低4B是Base2的b2成员，高4B未使用，第三个8B又是一个虚函数表指针，第四个8B低4B是Base1的b1成员高4B是Derive的c3成员，第五个8B又是一个虚函数指针，最后一个8B低四位是A的a成员，高4B未使用。注意这里只出现了一个A的副本，符合虚继承的情况。下面是三个虚函数表的内容。

```bash
虚表地址>0x5646fc2fcb80
第1个虚函数地址 :0Xfc0fc568,->Derive::func1
第2个虚函数地址 :0Xfc0fc492,->Base2::func2
第3个虚函数地址 :0Xfc0fc5a6,->Derive::func3

虚表地址>0x5646fc2fcbb0
第1个虚函数地址 :0Xfc0fc59f,->Derive::func1
第2个虚函数地址 :0Xfc0fc3d4,->Base1::func2

虚表地址>0x5646fc2fcbd8
第1个虚函数地址 :0Xfc0fc316,->A::fun1()
```

&emsp;但是这里出现了一个我暂时无法解释的现象，在不同的虚函数表对应的同一个虚函数的地址不一样。
&emsp;环境:Ubuntu18.04 LTS、g++ 5.5.0 。
