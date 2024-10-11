---
title: 流体模拟与SPH方法
top_img: 'https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/09/27/流体模拟与SPH方法/figure1.jpg'
comments: true
cover: 'https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/09/27/流体模拟与SPH方法/figure1.jpg'
toc: true
toc_number: true
copyright: true
date: 2021-09-27 16:46:17
updated:
tags: 
    - 流体模拟
    - SPH方法
    - 拉格朗日视角与欧拉视角
categories: 
    - [OpenSourceSummer2021]
    - [Computer Graphics]
    - [Physics engine]
keywords: 
    - 流体模拟
    - SPH方法
    - 拉格朗日视角与欧拉视角
description: 流体的物理模拟与SPH方法
---

<!--段首缩进
&emsp;
-->

<!--图片居中
<div align = center>
<img src="fig.PNG" width = "30%", height = "30%" />
</div>
-->

<!--多图片并排
<div align = center>
<img src="fig1.PNG" width = "30%", height = "30%" /><img src="fig2.PNG" width = "30%", height = "30%" />
</div>
-->

## 流体的物理模拟与SPH方法

&emsp;在引入有限元之前，先简单介绍相关的物理理论。在后面的部分，使用粗体符号如 $\boldsymbol{x}$ 表示向量或矩阵(张量), 未加粗的符号为标量, 如 $x$。

### 1、拉格朗日视角与欧拉视角

<div align = center>
<img src="https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/09/27/流体模拟与SPH方法/figure2.jpg" width = "60%", height = "60%" />
</div>

&emsp;烟雾、海浪、水滴…，这些司空见怪的自然现象其实有着非常复杂的数学规律，对于流体等连续介质材料的研究，有两种完全不同的视角，分别是欧拉视角和拉格朗日视角。

&emsp;欧拉视角的坐标系是固定的，如同站在河边观察河水的流动一样，用这种视角分析流体需要建立网格单元,使用网格将流体分成一个个小的“单元”，在这些单元上进行流体的物理模拟，计算相关的物理量等，这种思路与[有限元方法](https://tomsworkspace.github.io/2021/09/11/%E5%BC%B9%E6%80%A7%E6%9C%89%E9%99%90%E5%85%83%E6%96%B9%E6%B3%95/)联系了起来。而拉格朗日视角则将流体视为流动的单元，例如将一片羽毛放入风中，那么羽毛的轨迹可以帮我们指示空气的流动规律。

SPH算法是典型的拉格朗日视角，它的基本原理就是通过粒子模拟来流体的运动规律，然后再通过表面重建算法(Marching Cube)从粒子中生成网格来进行流体渲染。

### 2、SPH粒子受力分析

&emsp;SPH方法将流体看作大量粒子的集合，使用粒子的运动来模拟流体的流动，根据某点周围粒子的数量来计算流体在该点的压强、流速、密度等属性，同时又使用这些信息来更新粒子的运动情况。

&emsp;SPH算法的基本设想，就是将连续的流体想象成一个个相互作用的微粒，这些例子相互影响，共同形成了复杂的流体运动，对于每个单独的流体微粒，依旧遵循最基本的牛顿第二定律。流体的质量是由流体单元的密度决定的，所以在SPH中一般用密度代替质量来表示力: 

$$ \boldsymbol{F} = \rho \boldsymbol{a} \tag{1}$$

&emsp;作用在一个粒子上的作用力由三部分组成:

$$\boldsymbol{F} = \boldsymbol{F}^{external} + \boldsymbol{F}^{pressure} + \boldsymbol{F}^{viscosity}  \tag{2}$$

&emsp; 其中，$\boldsymbol{F}^{external}$ 是外力，一般就是重力，所以：

$$\boldsymbol{F}^{external} = \rho \boldsymbol{g}  \tag{3}$$

&emsp; $\boldsymbol{F}^{pressure}$ 是由流体内部的压力差产生的作用力，数值上等于压力场在这一点的梯度，力的方向有压力高的地方指向压力低的地方，所以：

$$\boldsymbol{F}^{pressure} = - \boldsymbol{\nabla p}  \tag{4}$$

&emsp; $\boldsymbol{F}^{viscosity}$ 是由于粒子间的速度差引起的力，设想在流动的液体内部，快速流动的部分会施加类似于剪切力的作用力到速度慢的部分，这个力的大小跟流体的粘度系数 $\mu$ 以及速度差有关，所以：

$$\boldsymbol{F}^{viscosity} = \mu\boldsymbol{\nabla^{2} u}  \tag{5}$$

这里的 $\boldsymbol{u}$ 是此处的速度， $\boldsymbol{\nabla^{2}}$ 是拉普拉辛算子，也叫二阶微分算子，有时也可写作 $\boldsymbol{\Delta}$或者$\boldsymbol{\nabla\cdot\nabla}$。

&emsp;由（1）-（5）,可以得到流体任一点粒子的加速度为:

$$\boldsymbol{a} = \boldsymbol{a^{external}} + \boldsymbol{a^{pressure}} + \boldsymbol{a^{viscosity}} = \boldsymbol{g} - \frac{\boldsymbol{\nabla p}}{\rho} + \frac{\mu\boldsymbol{\nabla^{2} u}}{\rho} \tag{6}$$

&emsp;根据这个公式，只要知道了某一点粒子的流体密度以及压强，就可以使用时间积分方法更新粒子的位置和速度了。下面来说流体某一点密度及压强的计算。

### 3、光滑核函数

&emsp;SPH方法将流体看作大量粒子的集合，使用粒子的运动来模拟流体的流动，根据某点周围粒子的数量来计算流体在该点的压强、流速、密度等属性，同时又使用这些信息来更新粒子的运动情况。可以这样理解这个概念，粒子的属性都会“扩散”到周围，我们使用一个函数来加权求和某一点的属性，这个函数要使得随着距离的增加对结果的影响逐渐变小，也就是在累加中的权重变小，这种随着距离而衰减的函数被称为“光滑核”函数，最大影响半径 $h$ 为“光滑核半径”。

<div align = center>
<img src="https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/09/27/流体模拟与SPH方法/figure3.jpg" width = "50%", height = "50%" />
</div>

&emsp;常用的核函数，比如Cubic soline kernel，高斯核等。

&emsp;设想流体中某点 $\boldsymbol{x}$（此处不一定有粒子）,在光滑核半径h范围内有数个粒子，位置分别是$\boldsymbol{x_0}$,$\boldsymbol{x_1}$,$\boldsymbol{x_2}$,...,$\boldsymbol{x_j}$，则该处某项属性 A (可以是压强 $p$、流速 $u$、密度 $rho$ 等属性)的累加公式为：

$$\boldsymbol{A}(x) = \sum_j \boldsymbol{A}_j \frac{m_j}{\rho_j}W(||\boldsymbol{x} - \boldsymbol{x_j}||_2, h) \tag{7}$$

&emsp;根据（7），某个属性在任意一点的梯度为：

$$\boldsymbol{\nabla A}(x) = \sum_j \boldsymbol{A}_j \frac{m_j}{\rho_j} \boldsymbol{\nabla_x} W(||\boldsymbol{x} - \boldsymbol{x_j}||_2, h) \tag{8}$$

&emsp; 某个属性在任意一点的二阶微分为：

$$\boldsymbol{\nabla^{2} A}(x) = \sum_j \boldsymbol{A}_j \frac{m_j}{\rho_j} \boldsymbol{\nabla^{2}_x} W(||\boldsymbol{x} - \boldsymbol{x_j}||_2, h) \tag{9}$$

&emsp;这里的 $\boldsymbol{\nabla^{2}_x} W$ 表示核函数在 $\boldsymbol{x}$ 处的二阶微分。

### 3、密度

&emsp;对于任一点的流体，其密度为：

$$\rho(x) = \sum_j \rho_j \frac{m_j}{\rho_j}W(||\boldsymbol{x} - \boldsymbol{x_j}||_2, h) = \sum_j m_j W(||\boldsymbol{x} - \boldsymbol{x_j}||_2, h) \tag{10}$$

### 4、压力

&emsp;对于任意某个粒子，产生的压强可以这样计算：

$$p_j = B ((\frac{\rho_j}{\rho_{j0}})^{\gamma} - 1) \tag{11}$$

&emsp;这里的 $B$ 是体积模量（Bulk module), $\rho$ 是粒子所在点的流体密度，$\gamma (\sim 7)$ 是一个常数。

&emsp;根据（8）, （6）中的压强产生的加速度部分可表示为：

$$\boldsymbol{a^{pressure}(x)} = - \frac{\boldsymbol{\nabla p(x)}}{\rho(x)} = - \frac{\boldsymbol{\sum_{j} p_j \frac{m_j}{\rho_j} \boldsymbol{\nabla_x} W(||\boldsymbol{x} - \boldsymbol{x_j}||_2, h)}}{\rho(x)} \tag{12}$$

&emsp;不过不幸的是，这个公式是“不平衡”的，也就是说，位于不同压强区的两个粒子之间的作用力不等，所以计算中一般使用双方粒子压强的算术平均值代替，任意处由压力产生的作用力的计算公式为：

$$\boldsymbol{a^{pressure}(x)} =  - \frac{\boldsymbol{\sum_{j} \frac{m_j(p(x) + p_j)}{2 \rho_j} \boldsymbol{\nabla_x} W(||\boldsymbol{x} - \boldsymbol{x_j}||_2, h)}}{\rho(x)} \tag{13}$$

### 4、粘度

&emsp;根据（8）, （7）中由粘度产生的加速度部分可表示为：

$$\boldsymbol{a^{viscosity}(x)} = \frac{\mu\boldsymbol{\nabla^{2} u(x)}}{\rho(x)} = \frac{\boldsymbol{\sum_{j} \boldsymbol{u_j} \frac{m_j}{\rho_j} \boldsymbol{\nabla^{2}_x} W(||\boldsymbol{x} - \boldsymbol{x_j}||_2, h)}}{\rho(x)} \tag{14}$$

&emsp;这个公式同样是“不平衡”的，由于速度是粒子间的相对速度，所以把它修正为：

$$\boldsymbol{a^{viscosity}(x)} = \frac{\boldsymbol{\sum_{j} \boldsymbol{u_j} m_j\frac{u_j - u(x)}{\rho_j} \boldsymbol{\nabla^{2}_x} W(||\boldsymbol{x} - \boldsymbol{x_j}||_2, h)}}{\rho(x)} \tag{15}$$

### 5、SPH方法

&emsp;综上所述，在每次迭代中，SPH的计算步骤为：

（1）根据公式（10）计算每个粒子处的密度。  
（2）根据公式（11）计算每个粒子处的压强。   
（3）根据公式（8）（13），计算所在位置的压强梯度及压强差产生的力的加速度。  
（4）根据公式（9）（15），计算所在位置的速度二阶微分及速度差产生的力的加速度。  
（5）根据公式（6），计算每个粒子的加速度。  
（6）使用[时间积分方法](https://tomsworkspace.github.io/2021/08/09/%E5%BC%B9%E7%B0%A7%E8%B4%A8%E7%82%B9%E7%B3%BB%E7%BB%9F%E4%B8%8E%E6%97%B6%E9%97%B4%E7%A7%AF%E5%88%86/)更新粒子速度和位置。如显式欧拉法：  

$$\boldsymbol v_{t+1} = \boldsymbol v_{t} + ∆t \boldsymbol a$$

$$\boldsymbol x_{t+1} = \boldsymbol x_{t} + ∆t \boldsymbol v_{t+1}$$

（7）根据更新后的粒子重建流体表面，渲染出流体。  

&emsp;具体地，SPH方法有许多变种，比如WCSPH, PCISPH, DFSPH等。都是基于以上的思路，在具体的公式上有一些细微的区别。具体可参考相应的论文。


### 参考资料

https://interactivecomputergraphics.github.io/SPH-Tutorial/

https://blog.csdn.net/liuyunduo/article/details/84098884

https://blog.csdn.net/qq_39300235/article/details/100982901

WCSPH: M. Becker and M. Teschner (2007). "Weakly compressible SPH for free surface flows". In:Proceedings of the 2007 ACM SIGGRAPH/Eurographics symposium on Computer animation. Eurographics Association, pp. 209–217.  

PCISPH: B. Solenthaler and R. Pajarola (2009). “Predictive-corrective incompressible SPH”. In: ACM SIGGRAPH 2009 papers, pp. 1–6.  

DFSPH: J. Bender, D. Koschier (2015) Divergence-free smoothed particle hydrodynamics[C] //Proceedings of the 14th ACM SIGGRAPH/Eurographics symposium on computer animation. ACM, 2015: 147-155.  

