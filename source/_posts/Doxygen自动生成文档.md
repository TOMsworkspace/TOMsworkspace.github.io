---
title: Doxygen自动生成文档
top_img: 'https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/08/06/Doxygen自动生成文档/Doxygen.png'
comments: true
cover: 'https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/08/06/Doxygen自动生成文档/Doxygen.png'
toc: true
toc_number: true
copyright: true
date: 2021-08-06 21:24:50
updated:
tags: Doxygen
categories: 
    - [OpenSourceSummer2021]
    - [文档开发]
keywords: 
    - Doxygen
    - Documentation
description: 使用Doxygen自动生成项目说明文档
---

## Doxygen 生成文档

### 1.简介

&emsp;我们在编写代码时一般会写一些注释，在写文档时又会用到这些注释。如果不能直接利用这些注释，就会做很多重复的工作。因此，Doxygen立足于解决这个问题，只要我们在写注释时按一定的格式来写，它就可以将我们在写代码时写的注释转化为各种格式的文档，已支持包括 HTML, LATEX, MAN pages, REF doc, XML, Docbook等多种格式。

&emsp;很多项目都使用了Doxygen来生成文档。比如：[LLVM](https://llvm.org/doxygen/), [CGAL](https://doc.cgal.org/4.2/CGAL.CGAL/html/index.html), [VTK](https://vtk.org/doc/nightly/html/index.html),glm,Eigen等。

&emsp;Doxygen生成的流程概述如下：
![format](https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/08/06/Doxygen自动生成文档/format.gif)

1.Doxygen

&emsp;Doxygen能将程序中的特定批注转换成为说明文件。它可以依据程序本身的结构，将程序中按规范注释的批注经过处理生成一个纯粹的参考手册，通过提取代码结构或借助自动生成的包含依赖图（include dependency graphs）、继承图（inheritance diagram）以及协作图（collaboration diagram）来可视化文档之间的关系。它支持多种语言，包括C, C++, python, java, c#, php, Objective-C, Fortran, VHDL, Markdown等。

2.graphviz

&emsp;Graphviz(Graph Visualization Software)是一个由AT&T实验室启动的开源工具包,用于绘制DOT语言脚本描述的图形。要使用Doxygen生成依赖图、继承图以及协作图，必须先安装graphviz软件。

3.HTML Help WorkShop

&emsp;微软出品的HTML Help WorkShop是制作CHM文件的最佳工具，它能将HTML文件编译生成CHM文档。Doxygen软件默认生成HTML文件或Latex文件，我们要通过HTML生成CHM文档，需要先安装HTML Help WorkShop软件，并在Doxygen中进行关联。

### 2. 安装及配置

安装及配置，参考[官方文档](https://www.doxygen.nl/manual/index.html)。

### 3. 如何写注释

&emsp; 要让Doxygen识别你的注释，需要让注释遵循一定的规范。简要的说，Doxygen注释块其实就是在C、C++注释块的基础添加一些额外标识,使Doxygen把它识别出来, 并将它组织到生成的文档中去。在每个代码项中都可以有两类描述：一种就是brief描述,另一种就是detailed。两种都是可选的，但不能同时没有。简述(brief)就是在一行内简述地描述。而详细描述(detailed)则提供更长,更详细的文档。在Doxygen中,主要通过以下方法将注释块标识成详细(detailed)描述:

&emsp;JavaDoc风格，在C风格注释块开始使用两个星号'*'：

```
/**
*  ... 描述 ...
*/
```
&emsp;Qt风格代码注释,即在C风格注释块开始处添加一个叹号'!':
```
/*!
* ... 描述 ...
*/
```
&emsp;使用连续两个以上C++注释行所组成的注释块, 而每个注释行开始处要多写一个斜杠或写一个叹号：
```
///
/// ... 描述 ...
///
```
&emsp;同样的，简要说明（brief）有也有多种方式标识，这里推荐使用@brief命令强制说明，例如

```
/** 
* @brief 简要注释Brief Description. 
*/
```

&emsp;注意以下几点：
```
1.Doxygen并不处理所有的注释，doxygen重点关注与程序结构有关的注释，比如：文件、类、结构、函数、全局变量、宏等注释，而忽略函数内局部变量、代码等的注释。  
2.注释应写在对应的函数或变量前面。JavaDoc风格下，自动会把第一个句号"."前的文本作为简要注释，后面的为详细注释。你也可以用空行把简要注释和详细注释分开。注意要设置JAVADOC_AUTOBRIEF或者QT_AUTOBRIEF设为YES。  
3.先从文件开始注释，然后是所在文件的全局函数、结构体、枚举变量、命名空间→命名空间中的类→成员函数和成员变量。  
4.Doxygen无法为DLL中定义的类导出文档。  
```

#### 3.1 注释实例

&emsp;下面用一个例子来总结一下常用的注释：

```
#ifndef TEST_H
#define TEST_H
文件头注释
/**
  * @file     	test.h
  * @author   	author
  * @email   	author@gmail.com
  * @version	V1.0
  * @date    	08-02-2021
  * @license  	GNU General Public License (GPL)  
  * @brief   	Universal Synchronous/Asynchronous Receiver/Transmitter(简要注释)
  * @detail	detail(详细描述)
  * @attention
  *  This file is part of OST.                                                  \n                                                                  
  *  This program is free software; you can redistribute it and/or modify       \n     
  *  it under the terms of the GNU General Public License version 3 as 		\n   
  *  published by the Free Software Foundation.                               	\n 
  *  You should have received a copy of the GNU General Public License   	\n      
  *  along with OST. If not, see <http://www.gnu.org/licenses/>.       		\n  
  *  Unless required by applicable law or agreed to in writing, software       	\n
  *  distributed under the License is distributed on an "AS IS" BASIS,         	\n
  *  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  	\n
  *  See the License for the specific language governing permissions and     	\n  
  *  limitations under the License.   											\n
  *   																			\n
  * @htmlonly 
  * <span style="font-weight: bold">History</span> 
  * @endhtmlonly 
  * Version|Auther|Date|Describe
  * ------|----|------|-------- 
  * V3.3|Jones Lee|07-DEC-2017|Create File
  * <h2><center>&copy;COPYRIGHT 2017 WELLCASA All Rights Reserved.</center></h2>
  */  

类注释
/**
* @class <class‐name> [header‐file] [<header‐name]
* @brief brief description(简要注释)
* @author <list of authors>
* @note
* detailed description(详细描述)
*/
class Test
{
public:
    /** @brief A enum, with inline docs */(简要注释)
    enum TEnum 
    {
        TVal1, /**< enum value TVal1. */  如果注释在每个成员后面。要在注释段中使用'<'标识。
        TVal2, /**< enum value TVal2. */ 
        TVal3  /**< enum value TVal3. */ 
    }

    *enumPtr, /**< enum pointer. */ 如果注释在每个成员后面。要在注释段中使用'<'标识。

    enumVar; /**< enum variable. */(简要注释)

    /** @brief A constructor. */ (简要注释)
    Test(); 

    /** @brief A destructor. */ 
    ~Test();

    /** @brief a normal member taking two arguments and returning an integer value. */ 
    int testMe(int a,const char *s); 

    函数/方法注释
    /**
    * @brief	can send the message(简要注释)
    * @param[in]	canx : The Number of CAN(参数说明)
    * @param[in]	id : the can id	
    * @param[in]	p : the data will be sent
    * @param[in]	size : the data size
    * @param[in]	is_check_send_time : is need check out the time out
    * @note	Notice that the size of the size is smaller than the size of the buffer.		
    * @return(返回值)		
    *	+1 Send successfully \n
    *	-1 input parameter error \n
    *	-2 canx initialize error \n
    *	-3 canx time out error \n
    * @par Sample
    * @code
    *	u8 p[8] = {0};
    *	res_ res = 0; 
    * 	res = can_send_msg(CAN1,1,p,0x11,8,1);
    * @endcode
    */							
    extern s32 can_send_msg(
        const CAN_TypeDef * canx,
        const u32 id,
        const u8 *p,
        const u8 size,
        const u8 is_check_send_time
    );
}
#ENDIF //TEST_H

```

#### 3.2 编辑器插件,自动注释补全

&emsp; 当然，这需要写大量的注释, 这是很烦的工作。尤其是注释之间是存在一些重复性的工作，比如注释相同的代码部分的注释段是格式相近的，比如类的注释，这是不是可以通过编辑器插件来解决呢？

&emsp; 当然，VScode就提供了这样的插件。在VScode里搜Doxygen Documentation Generator这个插件。安装一下，然后简单配置一下注释模板，即各个部分的注释风格和格式。然后就可以像代码补全一样很方便地写注释了。

<div align="center"> <img src="https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/08/06/Doxygen自动生成文档/vscodeExtend.png" /> </div>

<!-- ![Doxygen Documentation Generator](https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/08/06/Doxygen自动生成文档/vscodeExtend.png)-->

&emsp; 下面是一些简单用法。比如：生成一个文件的注释，

<div align="center"> <img src="https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/08/06/Doxygen自动生成文档/file.gif" /> </div>

<!--![file](https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/08/06/Doxygen自动生成文档/file.gif)-->

&emsp;生成一个方法/函数的注释：

<div align="center"> <img src="https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/08/06/Doxygen自动生成文档/alignment.gif" /> </div>

<!--![function](https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/08/06/Doxygen自动生成文档/alignment.gif)-->

&emsp;最后再修改一下注释成实际的内容就可以了。非常方便。