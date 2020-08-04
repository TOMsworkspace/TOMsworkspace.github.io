---
title: CPU Cache
comments: true
copyright: true
date: 2019-11-17 20:20:30
updated:
tags: 
	- cache
	- "CPU cache"
	- "三级缓存"
	- 缓存映射
	- cache原理
	- 多级cache
	- TLB
categories: "Linux kernel"
keywords: cache "CPU cache" "三级缓存" 缓存映射 cache原理 多级cache TLB
description: 描述CPU三级cache相关原理
top_img: cover.png
cover: https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/CPU-Cache/cover.png
toc: true
toc_number: false
---


# Linux的CPU cache

## 一．	CPU 与 Memory 内存之间的三级缓存的实现原理
### 1.1 cache 存在的原理
&emsp;&emsp;引入 Cache 的理论基础是程序局部性原理，包括时间局部性和空间局部性。时间局部性原理即最近被CPU访问的数据，短期内CPU 还要访问（时间）；空间局部性即被CPU访问的数据附近的数据，CPU短期内还要访问（空间）。因此如果将刚刚访问过的数据缓存在一个速度比主存快得多的存储中，那下次访问时，可以直接从这个存储中取，其速度可以得到数量级的提高。

&emsp;&emsp;CPU缓存是（Cache Memory）位于CPU与内存之间的临时存储器，它的容量比内存小但交换速度快。在缓存中的数据是内存中的一小部分，但这一小部分是短时间内CPU即将访问的，当CPU调用大量数据时，就可避开内存直接从缓存中调用，从而加快读取速度。

&emsp;&emsp;在CPU中加入缓存是一种高效的解决方案，是对于存储器件成本更低，速度更快这两个互相矛盾的目标的一个最优解决方案，这样整个内存储器（缓存+内存）就变成了既有缓存的高速度，又有内存的大容量的存储系统了。缓存对CPU的性能影响很大，主要是因为CPU的数据交换顺序和CPU与缓存间的带宽引起的。
下图是一个典型的存储器层次结构，我们可以看到一共使用了三级缓存
 
 ![](https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/CPU-Cache/figure1.jpg)

各级存储访问延迟的对比

 ![](https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/CPU-Cache/figure2.jpg)

### 1.2 cpu 三级cache
&emsp;&emsp;介于CPU和主存储器间的高速小容量存储器，由静态存储芯片SRAM组成，容量较小但比主存DRAM技术更加昂贵而快速， 接近于CPU的速度。CPU往往需要重复读取同样的数据块， Cache的引入与缓存容量的增大，可以大幅提升CPU内部读取数据的命中率，从而提高系统性能。通常由高速存储器、联想存储器、地址转换部件、替换部件等组成。如图所示。

![](https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/CPU-Cache/figure3.jpg)
 
- 联想存储器：根据内容进行寻址的存储器（冯氏模型中是按照地址进行寻址，但在高速存储器中往往只存有部分信息，此时需要根据内容进行检索）
- 地址转换部件：通过联想存储器建立目录表以实现快速地址转换。命中时直接访问Cache；未命中时从内存读取放入Cache
- 替换部件：在缓存已满时按一定策略进行数据块替换，并修改地址转换部件

&emsp;&emsp;早期采用外部（Off-chip）Cache，不做在CPU内而是独立设置一个Cache。现在采用片内（On-chip）Cache，将Cache和CPU作在一个芯片上，且采用多级Cache，同时使用L1 Cache和L2 Cache，甚至有L3 Cache。

- 一般L1 Cache都是分立Cache，分为数据缓存和指令缓存，可以减少访存冲突引起的结构冒险，这样多条指令可以并行执行；内置；其成本最高，对CPU的性能影响最大
多级Cache的情况下，L1 Cache的命中时间比命中率更重要
- 一般L2 Cache都是联合Cache，这样空间利用率高
没有L3 Cache的情况下，L2 Cache的命中率比命中时间更重要（缺失时需从主存取数，并要送L1和L2 cache）
* L3 Cache多为外置，在游戏和服务器领域有效；但对很多应用来说，总线改善比设置L3更加有利于提升系统性能

![](https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/CPU-Cache/figure4.jpg)
 
&emsp;&emsp;上图显示了最简单的缓存配置。它对应着最早期使用CPU cache的系统的架构。CPU内核不再直接连接到主内存。所有的数据加载和存储都必须经过缓存。CPU核心与缓存之间的连接是一种特殊的快速连接。在一个简化的表示中，主存和高速缓存连接到系统总线，该系统总线也可用于与系统的其他组件进行通信。我们引入了系统总线（现代叫做“FSB”）。
引入缓存后不久，系统变得更加复杂。高速缓存和主存之间的速度差异再次增大，使得另一个级别的高速缓存不得不被添加进来，它比第一级高速缓存更大且更慢。出于经济原因，仅增加第一级缓存的大小不是一种选择。今天，甚至有机器在生产环境中使用了三级缓存。带有这种处理器的系统如图下所示。随着单个CPU的内核数量的增加，未来的缓存级别数量可能会增加。现在已经出现了拥有四级cache的处理器了。

![](https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/CPU-Cache/figure5.jpg)

&emsp;&emsp;上图展示了三级缓存的结构。L1d是一级数据cache，L1i是一级指令cache。请注意，这只是一个示意图; 现实中的数据流从core到主存的过程中不需要经过任何更高级别的cache。CPU设计人员有很大的自由来设计cache的接口。对于程序员来说，这些设计选择是不可见的。
另外，我们有拥有多个core的处理器，每个core可以有多个“线程”。核心和线程之间的区别在于，独立的核心具有所有硬件资源的独立的副本，早期的多核处理器，甚至具有单独的第二级缓存而没有第三级缓存。核心可以完全独立运行，除非它们在同一时间使用相同的资源，例如与外部的连接。另一方面，线程们共享几乎所有的处理器资源。英特尔的线程实现只为线程提供单独的寄存器，甚至是有限的，还有一些寄存器是共享的。
一个现代CPU的完整概貌如图所示。

![一个 Intel Core i7处理器的Cache架构](https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/CPU-Cache/figure6.jpg)

- i-cache和d-cache都是32KB、8路、4个时钟周期；
- L2 cache：256KB 、8路、11个时钟周期。
- 所有核共享的L3 cache：8MB、16路、30~40个时钟周期。
- Core i7中所有cache的块大小都是64B

### 1.3 cpu cache与TLB的联系
&emsp;&emsp;由于cache中对应的都是主存地址，即物理地址，在cqu查看具体数据是否在cache中时，如果CPU传送过来的地址时一个虚拟地址，需要将其转换成实际物理地址再到cache中去寻找。Cache的实现需要TLB的帮助。可以说TLB命中是Cache命中的基本条件。TLB不命中，会更新TLB项，这个代价非常大，Cache命中的好处基本都没有了。在TLB命中的情况下，物理地址才能够被选出，Cache的命中与否才能够达成。

#### 1.3.1 TLB的概述

&emsp;&emsp;TLB是一个内存管理单元用于改进虚拟地址到物理地址转换速度的缓存。TLB是位于内存中的页表的cache，如果没有TLB，则每次取数据都需要两次访问内存,即查页表获得物理地址和取数据。

#### 1.3.2 TLB的原理

&emsp;&emsp;当cpu对数据进行读请求时,CPU根据虚拟地址(前20位)到TLB中查找.TLB中保存着虚拟地址(前20位)和页框号的对映关系,如果匹配到虚拟地址就可以迅速找到页框号(页框号可以理解为页表项),通过页框号与虚拟地址后12位的偏移组合得到最终的物理地址.

&emsp;&emsp;如果没在TLB中匹配到虚拟地址,就出现TLB丢失,需要到页表中查询页表项,如果不在页表中,说明要读取的内容不在内存,需要到磁盘读取.

&emsp;&emsp;TLB是MMU中的一块高速缓存,也是一种Cache.在分页机制中,TLB中的数据和页表的数据关联,不是由处理器维护,而是由OS来维护,TLB的刷新是通过装入处理器中的CR3寄存器来完成.如果MMU发现在TLB中没有命中,它在常规的页表查找后,用找到的页表项替换TLB中的一个条目.

#### 1.3.3 TLB的刷新原则

&emsp;&emsp;当进程进行上下文切换时重新设置cr3寄存器,并且刷新tlb.
有两种情况可以避免刷tlb.
 第一种情况是使用相同页表的进程切换.
 第二种情况是普通进程切换到内核线程.
lazy-tlb(懒惰模式)的技术是为了避免进程切换导致tlb被刷新.
当普通进程切换到内核线程时,系统进入lazy-tlb模式,切到普通进程时退出该模式.

#### 1.3.4 cache的概念

&emsp;&emsp;cache是为了解决处理器与慢速DRAM(慢速DRAM即内存)设备之间巨大的速度差异而出现的。cache属于硬件系统,linux不能管理cache.但会提供flush整个cache的接口.
cache分为一级cache,二级cache,三级cache等等.一级cache与cpu处于同一个指令周期.

#### 1.3.5 Cache的存取单位(Cache Line)

&emsp;&emsp;CPU从来不从DRAM直接读/写字节或字,从CPU到DRAM的每次读或写的第一步都要经过L1 cache,每次以整数行读或写到DRAM中.Cache Line是cache与DRAM同步的最小单位.典型的虚拟内存页面大小为4KB,而典型的Cache line通常的大小为32或64字节.

&emsp;&emsp;CPU 读/写内存都要通过Cache,如果数据不在Cache中,需要把数据以Cache Line为单位去填充到Cache,即使是读/写一个字节.CPU 不存在直接读/写内存的情况,每次读/写内存都要经过Cache.

#### 1.3.6 Cache的工作模式

- 数据回写(write-back):这是最高性能的模式,也是最典型的,在回写模式下,cache内容更改不需要每次都写回内存,直到一个新的 cache要刷新或软件要求刷新时,才写回内存.
- 写通过(write-through):这种模式比回写模式效率低,因为它每次强制将内容写回内存,以额外地保存cache的结果,在这种模式写耗时,而读和回写模一样快,这都为了内存与cache相一致而付出的代价.
- 预取 (prefectching):一些cache允许处理器对cache line进行预取,以响应读请求,这样被读取的相邻内容也同时被读出来,如果读是随机的,将会使CPU变慢,预取一般与软件进行配合以达到最高性能.

## 二. 各级缓存中数据的包含关系

### 2.1 整个缓存和主存间
&emsp;&emsp;缓存里有的数据，主存中一定存在。
### 2.2 各级缓存之间

&emsp;&emsp;一级缓存中还分数据缓存（data cache，d-cache）和指令缓存（instruction cache，i-cache）。二者分别用来存放数据和执行这些数据的指令，而且两者可以同时被cpu访问，所以一级cache间数据时独立的。

&emsp;&emsp;一级没有的数据二级可能有也可能没有。因为一级缓存miss会接着访问二级缓存。

&emsp;&emsp;一级有二级一定有，三级也一定有。因为一级的数据从二级中读上来的。在一级缺失二级命中时发生。

&emsp;&emsp;二级没有的数据三级可能有也可能没有。因为二级确实会接着访问三级缓存。找不到会继续访问主存。

&emsp;&emsp;二级有的数据三级一定有。在二级缺失三级命中时，数据会从三级缓存提到二级缓存。

&emsp;&emsp;三级没有的数据，主存可能有也可能没有。三级缓存缺失，会访问主存，主存也缺失就要从外存访问数据了。

&emsp;&emsp;三级缓存有的数据主存一定有。因为在三级缺失主存命中时，数据会从主存提到三级缓存中来。

## 三. 各级缓存的大小设置

&emsp;&emsp;一级缓存就是指CPU第一层级的高速缓存，主要是为了缓存指令和缓存数据，一级缓存的容量对CPU性能影响非常大，但是因为成本太高，所以一般容量特别小，也就256KB左右。

&emsp;&emsp;二级缓存是CPU第二层级的高速缓存，对于CPU来说，二级缓存容量越大越好，它是直接影响CPU性能的，CPU每个核心都会有自己的缓存，一个CPU的二级缓存容量是所有核心二级缓存容量的总和。

&emsp;&emsp;三级缓存就是CPU第三层级的高速缓存，主要是为了降低与内存进行数据传输时的延迟问题，三级缓存与一二级不同，三级缓存只有一个，它是所有核心共享，所以在CPU参数中可以看到，三级缓存相对于其他两级缓存来说都很大。

&emsp;&emsp;**由于缓存的设置与OS无关且透明，所以对于不同的体系架构下不同的处理器对待缓存区域的处理和方式都不同，不同的处理器也有不同的缓存设置值。从主流的处理器cache大小来看，一般一个cache line的大小都是固定的64B左右，这是经过经验得到的比较合理的大小，一般一级cache大小在数十KB左右，二级cache大小在数十到数百KB左右，而L3 cache大小在数MB左右。**

## 四. 各级缓存之间的数据放置与数据淘汰策略

&emsp;&emsp;由于三级cache一般来说是运用于拥有多核的处理器，对于单核处理器来说二级cache就能够足够保持够高的cache命中率。所以一般的三级cache一般只针对于多核处理器。L1和L2级cache是处理器核所单独的内容。L1又可以看成是L2的cache。L2可以看成是L3级cache的cache。所以我们分两个部分讨论数据放置与数据淘汰策略。

### 4.1 各级cache之间数据放置方式
&emsp;&emsp;各级cache间的数据放置策略主要有三种。直接相连映射，全相联映射和组相联映射。将一个主存块存储到唯一的一个Cache行。对应的大小都是一个cache line的大小，一般来说是64B。
#### 4.1.1直接相连映射
&emsp;&emsp;多对一的映射关系，但一个主存块只能拷贝到cache的一个特定行位置上去。cache的行号i和主存的块号j有如下函数关系：i=j mod m（m为cache中的总行数）。

![](https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/CPU-Cache/figure7.jpg)
 
- 优点：硬件简单，容易实现
- 缺点：命中率低， Cache的存储空间利用率低

#### 4.1.2全相联映射
&emsp;&emsp;可以将一个主存块存储到任意一个Cache行。
主存的一个块直接拷贝到cache中的任意一行上

![](https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/CPU-Cache/figure8.jpg)
 
- 优点：命中率较高，Cache的存储空间利用率高
- 缺点：线路复杂，成本高，速度低

#### 4.1.3组相联映射
&emsp;&emsp;可以将一个主存块存储到唯一的一个Cache组中任意一个行。
将cache分成u组，每组v行，主存块存放到哪个组是固定的，至于存到该组哪一行是灵活的，即有如下函数关系：cache总行数m＝u×v 组号q＝j mod u

![](https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/CPU-Cache/figure9.jpg) 

&emsp;&emsp;组间采用直接映射，组内为全相联。硬件较简单，速度较快，命中率较高。是现代处理器中一般所常用的映射方式。

### 4.2 数据淘汰策略
&emsp;&emsp;Cache工作原理要求它尽量保存最新数据，当从主存向Cache传送一个新块，而Cache中可用位置已被占满时，就会产生Cache替换的问题。 
常用的替换算法有下面三种。 
#### 4.2.1 LFU 
&emsp;&emsp;LFU（Least Frequently Used，最不经常使用）算法将一段时间内被访问次数最少的那个块替换出去。每块设置一个计数器，从0开始计数，每访问一次，被访块的计数器就增1。当需要替换时，将计数值最小的块换出，同时将所有块的计数器都清零。 
这种算法将计数周期限定在对这些特定块两次替换之间的间隔时间内，不能严格反映近期访问情况，新调入的块很容易被替换出去。 
#### 4.2.2 LRU 
&emsp;&emsp;LRU（Least Recently Used，近期最少使用）算法是把CPU近期最少使用的块替换出去。这种替换方法需要随时记录Cache中各块的使用情况，以便确定哪个块是近期最少使用的块。每块也设置一个计数器，Cache每命中一次，命中块计数器清零，其他各块计数器增1。当需要替换时，将计数值最大的块换出。 
&emsp;&emsp;LRU算法相对合理，但实现起来比较复杂，系统开销较大。这种算法保护了刚调入Cache的新数据块，具有较高的命中率。LRU算法不能肯定调出去的块近期不会再被使用，所以这种替换算法不能算作最合理、最优秀的算法。但是研究表明，采用这种算法可使Cache的命中率达到90%左右。 
#### 4.2.3 随机替换 
&emsp;&emsp;最简单的替换算法是随机替换。随机替换算法完全不管Cache的情况，简单地根据一个随机数选择一块替换出去。随机替换算法在硬件上容易实现，且速度也比前两种算法快。缺点则是降低了命中率和Cache工作效率。

## 五. 整个缓存结构的访问流程
### 5.1 查找命中
&emsp;&emsp;处理器微架构访问Cache的方法与访问主存储器有类似之处。主存储器使用地址编码方式，微架构可以地址寻址方式访问这些存储器。Cache也使用了类似的地址编码方式，微架构也是使用这些地址操纵着各级Cache，可以将数据写入Cache，也可以从Cache中读出内容。只是这一切微架构针对Cache的操作并不是简单的地址访问操作。为简化起见，我们忽略各类Virtual Cache，讨论最基础的Cache访问操作，并借此讨论CPU如何使用TLB完成虚实地址转换，最终完成对Cache的读写操作。

![](https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/CPU-Cache/figure10.jpg)
 
&emsp;&emsp;Cache的存在使得CPU Core的存储器读写操作略微显得复杂。CPU Core在进行存储器方式时，首先使用EPN(Effective Page Number)进行虚实地址转换，并同时使用CLN(Cache Line Number)查找合适的Cache Block。这两个步骤可以同时进行。在使用Virtual Cache时，还可以使用虚拟地址对Cache进行寻址。为简化起见，我们并不考虑Virtual Cache的实现细节。

&emsp;&emsp;EPN经过转换后得到VPN，之后在TLB中查找并得到最终的RPN(Real Page Number)。如果期间发生了TLB Miss，将带来一系列的严重的系统惩罚，我们只讨论TLB Hit的情况，此时将很快获得合适的RPN，并依此得到PA(Physical Address)。

&emsp;&emsp;在多数处理器微架构中，Cache由多行多列组成，使用CLN进行索引最终可以得到一个完整的Cache Block。但是在这个Cache Block中的数据并不一定是CPU Core所需要的。因此有必要进行一些检查，将Cache Block中存放的Address与通过虚实地址转换得到的PA进行地址比较(Compare Address)。如果结果相同而且状态位匹配，则表明Cache Hit。此时微架构再经过Byte Select and Align部件最终获得所需要的数据。如果发生Cache Miss，CPU需要使用PA进一步索引主存储器获得最终的数据。

&emsp;&emsp;由上文的分析，我们可以发现，一个Cache Block由预先存放的地址信息，状态位和数据单元组成。一个Cache由多个这样的Cache Block组成，在不同的微架构中，可以使用不同的Cache Block组成结构。我们首先分析单个Cache Block的组成结构。单个Cache Block由Tag字段，状态位和数据单元组成，如图所示。

![](https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/CPU-Cache/figure11.jpg)
 
&emsp;&emsp;其中Data字段存放该Cache Block中的数据，在多数处理器微架构中，其大小为32或者64字节。Status字段存放当前Cache Block的状态，在多数处理器系统中，这个状态字段包含MESI，MOESI或者MESIF这些状态信息，在有些微架构的Cache Block中，还存在一个L位，表示当前Cache Block是否可以锁定。许多将Cache模拟成SRAM的微架构就是利用了这个L位。有关MOESIFL这些状态位的说明将在下文中详细描述。在多核处理器和复杂的Cache Hierarchy环境下，状态信息远不止MOESIF。

&emsp;&emsp;RAT(Real Address Tag)记录在该Cache Block中存放的Data字段与那个地址相关，在RAT中存放的是部分物理地址信息，虽然在一个CPU中物理地址可能有40，46或者48位，但是在Cache中并不需要存放全部地址信息。因为从Cache的角度上看，CPU使用的地址被分解成为了若干段，如图所示。

![](https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/CPU-Cache/figure12.jpg)

&emsp;&emsp;这个地址也可以理解为CPU访问Cache使用的地址，由多个数据段组成。首先需要说明的是Cache Line Index字段。这一字段与Cache Line Number类似，CPU使用该字段从Cache中选择一个或者一组Entry。

&emsp;&emsp;Bank和Byte字段之和确定了单个Cache的Data字段长度，通常也将这个长度称为Cache 行长度，上图所示的微架构中的Cache Block长度为64字节。目前多数支持DDR3 SDRAM的微架构使用的Cache Block长度都是64字节。部分原因是由于DDR3 SDRAM的一次Burst Line为8，一次基本Burst操作访问的数据大小为64字节。

&emsp;&emsp;在处理器微架构中，将地址为Bank和Byte两个字段出于提高Cache Block访问效率的考虑。Multi-Bank Mechanism是一种常用的提高访问效率的方法，采用这种机制后，CPU访问Cache时，只要不是对同一个Bank进行访问，即可并发执行。Byte字段决定了Cache的端口位宽，在现代微架构中，访问Cache的总线位宽为64位或者为128位。

&emsp;&emsp;剩余的字段即为Real Address Tag，这个字段与单个Cache中的Real Address Tag的字段长度相同。CPU使用地址中的Real Address Tag字段与Cache Block的对应字段和一些状态位进行联合比较，判断其访问数据是否在Cache中命中

### 5.2 cache 不命中

&emsp;&emsp;如果cache miss,就去下一级cache或者主存中去查找数据，并将查找到的数据采用上面的数据淘汰策略将数据替换到cache中。

### 5.3 cache和主存数据一致性保持

&emsp;&emsp;由于在发生cache miss时会产生数据替换，在运行过程中缓存的数据也可能会被修改。所以需要一个策略来保持数据在缓存和主存间的一致性。
Cache写机制分为write through和write back两种。

- Write-through（直写模式）在数据更新时，同时写入缓存Cache和主存存储。此模式的优点是操作简单；缺点是因为数据修改需要同时写入存储，数据写入速度较慢。

- Write-back（回写模式）在数据更新时只写入缓存Cache。只在数据被替换出缓存时，被修改的缓存数据才会被写到后端存储。此模式的优点是数据写入速度快，因为不需要写存储；缺点是一旦更新后的数据未被写入存储时出现系统掉电的情况，数据将无法找回。

