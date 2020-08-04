---
title: DLL-lose
comments: true
copyright: true
date: 2019-11-21 15:55:47
updated:
tags:
- Dll文件丢失
categories: 其他
keywords: Dll丢失
description: 解决计算机程序中出现.DLL文件丢失的问题
top_img: https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/DLL-lose/dll_lose1.jpg
cover: https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/DLL-lose/dll_lose1.jpg
toc: true
toc_number: false
---

&emsp;&emsp;在安装某些软件，我们正准备开开心心地打开，哦豁，duang的一声弹出一个框框。就像下面这样。

![dll-lose](https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/DLL-lose/dll_lose2.jpg)

这时候是不是一筹莫展呢？别灰心，这类问题大多数还是能解决的。
<!--more-->

## 1. DLL文件的概念
### 1.1 什么是dll文件
&emsp;&emsp;DLL(Dynamic Link Library)文件为动态链接库文件，又称“应用程序拓展”，是软件文件类型。在Windows中，许多应用程序并不是一个完整的可执行文件，它们被分割成一些相对独立的动态链接库，即DLL文件，放置于系统中。当我们执行某一个程序时，相应的DLL文件就会被调用。一个应用程序可使用多个DLL文件，一个DLL文件也可能被不同的应用程序使用，这样的DLL文件被称为共享DLL文件。

&emsp;&emsp;在 Windows操作系统中，每个程序都可以使用该 DLL 中包含的功能来实现“打开”对话框。

&emsp;&emsp;DLL文件中存放的是各类程序的函数(子过程)实现过程，当程序需要调用函数时需要先载入DLL，然后取得函数的地址，最后进行调用。使用DLL文件的好处是程序不需要在运行之初加载所有代码，只有在程序需要某个函数的时候才从DLL中取出。这有助于促进代码重用和内存的有效使用。

### 1.2 使用dll文件的好处
- **实现模块化**

&emsp;&emsp;通过使用 DLL，程序可以实现模块化，由相对独立的组件组成。例如，一个记账程序可以按模块来销售。可以在运行时将各个模块加载到主程序中（如果安装了相应模块）。因为模块是彼此独立的，所以程序的加载速度更快，而且模块只在相应的功能被请求时才加载。另外，使用DLL文件还可以减小程序的体积。

- **便于应用更新**

&emsp;&emsp;可以更为容易地将更新应用于各个模块，而不会影响该程序的其他部分。例如，您可能具有一个工资计算程序，而税率每年都会更改。当这些更改被隔离到 DLL 中以后，您无需重新生成或安装整个程序就可以应用更新。

## 2. dll文件丢失的解决办法
&emsp;&emsp;当某个程序或 DLL 使用其他 DLL 中的 DLL 函数时，就会创建依赖项。因此，该程序就不再是独立的，并且如果该依赖项被损坏，该程序就可能遇到问题。就会产生类似于.dll文件丢失这样的问题。 

### 2.1 dll文件丢失的原因
&emsp;&emsp;出现DLL文件丢失一般出现在Windows系统中。产生dll文件丢失的原因有很多。大概总结了一下，有以下的几种：
	（1）程序依赖的 DLL文件升级到新版本
	（2）未安装程序需要的DLL文件
	（3）依赖 DLL 被其早期版本覆盖
	（4）从计算机中删除了依赖 DLL
	（5）由dll文件命名引发的丢失

### 2.2 解决办法
&emsp;&emsp;解决dll丢失问题的方法有一下几种，不过并不是所有的解决方法都能解决问题。在选择解决问题的方法之前先找到产生丢失dll的具体原因是什么，还有丢失的dll文件是什么类型的。然后再对症下药，方能药到病除。

**丢失文件的类型：**

&emsp;&emsp;丢失的dll文件是与编程语言和系统环境有关的dll文件。一般出现在microsoft自己的软件运行时出现，比如许多微软自己开发的开发工具，VS ，vc++,Qt之类的程序，可能的原因是(1)(2)(3)(4)。

&emsp;&emsp;丢失的dll文件是与具体程序相关的，非microsoft相关的，一般出现在一个刚安装的程序或者不需要安装可以直接运行的exe文件运行时出现的。还有就是网上下载的所谓的破解版的软件最容易出现这种问题。出现的原因可能是(2)(4)。

**解决方法：**

**方法1：**下载一键式修复工具

&emsp;&emsp;有许多人开发了专门针对这类dll丢失问题的一键式修复工具。如 [Diretx工具](http://www.pc6.com/softview/SoftView_57945.html)

&emsp;&emsp;这是一个一键批量检测当前系统丢失的dll文件并进行自动修复，使用方法是最简单的。只能解决第一类问题中的少部分问题，可以用来修复那些系统相关的dll文件。[使用方法](https://blog.csdn.net/qq_38796548/article/details/88842013)

**方法2：**下载丢失的对应的DLL文件并放到对应的目录

&emsp;&emsp;将dll文件复制到Windows系统目录或者复制到程序安装目录中。针对报丢失的dll文件，按照名字去搜索对应的DLL文件下载，并放置在对应的目录。一般第一类的问题，和系统相关的dll文件放在系统对应的目录下。(32位系统在 C:\Windows\System32,64位系统放在C：\Windows\SysWOW64下）和程序相关的放在对应程序安装目录下。一般是这样，但是也不是绝对的，也有的程序丢失的dll放在系统目录下的，比如有的.exe程序。

&emsp;&emsp;下面给出一些可以搜索下载dll文件的网址：

- [Dll-files](https://cn.dll-files.com/)
- <https://www.zhaodll.com/>
- [Dll之家](http://www.dllzj.com/)
- [脚本之家](https://www.jb51.net/dll/)
- [DLL文件下载器](https://pan.baidu.com/s/1rPsC3MffUmLcdtgONs0t0A)或者<https://gitee.com/wyatthuang/dll_downloader>

&emsp;&emsp;这个是一个爬取工具。原理是通过Python的urllib库，爬取DLL共享网站https://cn.dll-files.com, 并下载dll文件。
软件运行后，按照提示提示搜索下载就可以了，非常很简单。

**方法3：**在对应的目录检查一下文件命名的问题

&emsp;&emsp;这个问题一般不会出现，一般由于自己下载的dll文件已经放在对应目录下但是由于命名的原因没有识别到。

**方法4：**重新安装程序

&emsp;&emsp;重新执行出现问题程序的安装程序，重新安装来解决dll丢失问题。不过对于系统dll文件丢失和.exe程序没有作用。

**方法5：**使用Windows系统文件检查器修复.dll错误（sfc / scannow）

<https://www.reneelab.com.cn/windows-10-sfc-scannow.html>

**方法6：**重启大法

&emsp;&emsp;在使用多种方法不起效或者使用重装大法之前，可以使用重启系统试试。重启可以让没启用的配置生效，或许可以解决你的问题。

**方法7：**通过重装或者更新Windows操作系统来摆脱dll错误

&emsp;&emsp;这个方法成本太大了，不建议使用

一般来说，大多数问题通过这些方法都是可以解决的。如果还有的话，请留言告诉我一声。哈哈
