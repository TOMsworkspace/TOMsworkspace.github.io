---
title: 通过修改注册表来更改IE代理
comments: true
copyright: true
date: 2019-12-22 16:16:39
updated:
tags: 
 - 修改注册表
 - 更改IE代理
 - IE代理设置
categories: 其他
keywords: 修改注册表 更改IE代理 IE代理设置
description: 通过修改注册表来更改IE浏览器的代理设置
top_img:
cover:
toc: true
toc_number: true
---

# 通过修改注册表来更改IE浏览器的代理设置

## 一、什么是注册表
&emsp;注册表是windows操作系统、硬件设备以及客户应用程序得以正常运行和保存设置的核心“数据库”，也可以说是一个非常巨大的树状分层结构的数据库系统。  
&emsp;注册表记录了用户安装在计算机上的软件和每个程序的相互关联信息，它包括了计算机的硬件配置，包括自动配置的即插即用的设备和已有的各种设备说明、状态属性以及各种状态信息和数据。利用一个功能强大的注册表数据库来统一集中地管理系统硬件设施、软件配置等信息，从而方便了管理，增强了系统的稳定性。  

## 二、注册表的功能
&emsp;注册表是windows操作系统中的一个核心数据库，其中存放着各种参数，直接控制着windows的启动、硬件驱动程序的装载以及一些windows应用程序的运行，从而在整个系统中起着核心作用。这些作用包括了软、硬件的相关配置和状态信息，比如注册表中保存有应用程序和资源管理器外壳的初始条件、首选项和卸载数据等，联网计算机的整个系统的设置和各种许可，文件扩展名与应用程序的关联，硬件部件的描述、状态和属性，性能记录和其他底层的系统状态信息，以及其他数据等。  
&emsp;具体来说，在启动Windows时，Registry会对照已有硬件配置数据，检测新的硬件信息；系统内核从Registry中选取信息，包括要装入什么设备驱动程序，以及依什么次序装入，内核传送回它自身的信息，例如版权号等；同时设备驱动程序也向Registry传送数据，并从Registry接收装入和配置参数，一个好的设备驱动程序会告诉Registry它在使用什么系统资源，例如硬件中断或DMA通道等，另外，设备驱动程序还要报告所发现的配置数据；为应用程序或硬件的运行提供增加新的配置数据的服务。配合ini文件兼容16位Windows应用程序，当安装—个基于Windows 3.x的应用程序时，应用程序的安装程序Setup像在windows中—样创建它自己的INI文件或在win.ini和system.ini文件中创建入口；同时windows还提供了大量其他接口，允许用户修改系统配置数据，例如控制面板、设置程序等。    
&emsp;如果注册表受到了破坏，轻则使windows的启动过程出现异常，重则可能会导致整个windows系统的完全瘫痪。因此正确地认识、使用，特别是及时备份以及有问题恢复注册表对windows用户来说就显得非常重要。


## 如何打开注册表
&emsp;打开注册表的命令是：

```win+R

regedit或regedit.exe、regedt32或regedt32.exe   
```

&emsp;正常情况下，你可以点击开始菜单当中的运行，然后输入regedit或regedit.exe点击确定就能打开windows操作系统自带的注册表编辑器了，有图慎重提醒，操作注册表有可能造成系统故障，若您是对windows注册表不熟悉、不了解或没有经验的windows操作系统用户建议尽量不要随意操作注册表。

&emsp;如果上述打开注册表的方法不能使用，说明你没有管理员权限，或者注册表被锁定，如果是没有权限，请寻找电脑管理员帮助解决，如果注册表被锁定，请参照下面的方式进行解锁。

## 注册表解锁常见的方法：
&emsp;**1**：创建一个文本文件，复制以下文字文本内容（注意开头之后第二行一定要是空行并且不可少），选择另存为，文件类型选择所有文件，文件名称为注册表解锁.reg  
```
REGEDIT4
[HKEY_USERS.DEFAULT\Software\Microsoft\Windows\CurrentVersion\Policies\system"DisableRegistryTools"=dword:00000000]
```
保存文件到桌面，双击打开桌面上的注册表解锁.reg如下图，点击确定即可。

&emsp;**2**：使用第三方工具恢复，比如使用超级兔子或者优化大师这类系统辅助软件，
以下以优化大师为例说明：
打开优化大师，点击左侧的系统优化，然后选择系统安全优化，点击右侧的更多设置，，取消禁用注册表编辑器项目前面的对勾。

&emsp;**3**：利用系统策略编辑器
在Windows 2000/XP/2003操作系统下
在Windows 2000/XP/2003等操作系统当中，我们可以通过单击 开始-运行，输入gpedit.msc之后点击确定或按回车，打开windows操作系统自带的组策略编辑器。然后，依次展开用户配置-管理模板-系统，双击右侧窗口中的阻止访问注册表编辑工具，在弹出的窗口中选择已禁用，确定后再退出组策略编辑器，即可为注册表解锁。

## 如何修改浏览器的IE设置

&emsp;一般来说，可以通过系统系带的图形界面来更改IE的代理服务器设置

&emsp;对于Win10系统，打开设置，选择网络和internet,再选择左下角的代理就可以进行设置。
或者再网络图标点击右键打开网络和Internet设置，选择代理进行设置。

&emsp;对于更低版本的Windows系统，在网络图标上点击右键打开网络和Internet设置，在搜索栏里搜索代理，就可以进行代理服务器的设置。

也可以直接在浏览器的设置菜单里实现对代理的修改

可是有的时候，我们就需要运用其他方式对代理进行更改。于是可以选择更改注册表来实现ie代理的更改。有两种方法：

### 通过自动生成注册表文件，再调用reg.exe命令

&emsp;在说明本文之前，首先说明有比较简单的方法，java程序自动生成注册表文件*.reg，格式为：  
```
REGEDIT4
[HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings]
"ProxyEnable"=dword:00000001
"ProxyServer"="localhost:8080"
"ProxyOverride"="<localhost>"
```
然后调用，Runtime.getRuntime().exec( "reg.exe import " + filename);

### 通过调用registry的api修改
&emsp;既然目前已经有现成的修改注册表的包jni的registry包，那我们就不费这个事做上面的操作了，registry jar包[下载地址](http://www.trustice.com/java/jnireg/index.shtml)，另外发现openorg上也有类似的，但是类名改掉了，感兴趣的同学可以去搜搜。

IE代理服务器对应于注册表中字段：HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings下面的值：ProxyServer，ProxyEnable，ProxyOverride
ProxyEnable用来表示是否使用代理，用0,1表示，类型为REG_DWORD，不能为REG_SZ
ProxyServer用来表示代理服务器ip:port，如localhost:8080，类型为REG_SZ
ProxyOverride表示跳过代理的配置，比如跳过本地代理，该值为<local>


原文链接：<https://blog.csdn.net/iteye_21060/article/details/82212107>

### 通过bat脚本设置系统代理，然后在java中调用bat

java的Runtime.getRuntime().exec(commandStr)可以调用执行cmd指令。
通过他来调用运行bat脚本的cmd命令来实现更改。

原文链接：<https://blog.csdn.net/baidu_23275675/article/details/84427757>

## 修改注册表带来的一个问题

&emsp;通过修改注册表来实现对代理设置的更改不会立即生效，这是注册表本身自带的缺陷。对于这个问题也是有解决办法的：[如何使更改注册表立即生效](https://tomsworkspace.github.io/2019/12/22/%E4%BF%AE%E6%94%B9Windows%E7%B3%BB%E7%BB%9F%E6%B3%A8%E5%86%8C%E8%A1%A8%E5%B9%B6%E4%BD%BF%E5%85%B6%E7%AB%8B%E5%8D%B3%E7%94%9F%E6%95%88/)




