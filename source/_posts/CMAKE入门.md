---
title: CMAKE入门
top_img: 'https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/05/24/CMAKE入门/CMake-Logo.png'
comments: true
cover: 'https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/05/24/CMAKE入门/CMake-Logo.png'
toc: true
toc_number: true
copyright: true
date: 2021-05-24 19:03:56
updated:
tags: CMAKE
categories: 
    - [OpenSourceSummer2021]
    - [CMAKE]
keywords: CMAKE入门
description:
---

## What CMake can do 

### 跨平台构建

&emsp; 一套C/C++代码，多平台运行。假设在Windows上, OSX和Linux上使用：Visual Studio, Xcode, Makefile.可以一套代码基于同一个CMAKE即时编译。直接生成项目，不需要额外配置。

![Cross-platform development](https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/05/24/CMAKE入门/native-build.png)

### VCS友好

&emsp;当项目出现更新，如添加一个新文件。这个工作如果交给IDE来做，很麻烦。交给CMAKE，只需要一行代码，类似于Makefile做的。

![VCS friendly](https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/05/24/CMAKE入门/generate-native-files-add.png)

### 多生成环境支持

&emsp;CMAKE已经开始支持多种IDE工具，可以直接通过CMAKE生成IED对应的项目，当切换IDE进行开发时，只需要简单一步即可构建。可直接生成VS项目、xcode项目，eclipse项目、各种平台的Makefile等。

&emsp;CMAKE现已支持如下的IDE及开发环境。可通过
```bash
    cmake -help
```
来查看。

### 全流程支持

&emsp;从开发到调试，从生成到构建，从编译到测试，从打包到安装全流程覆盖。

![All stage development cover](https://cdn.jsdelivr.net/gh/TOMsworkspace/TOMsworkspace.github.io/2021/05/24/CMAKE入门/cmake-environment.png)

## HOW TO LEARN CMAKE

&emsp; [官方文档](https://cmake.org/documentation/)

&emsp; [Microsoft](https://docs.microsoft.com/en-us/cpp/build/cmake-projects-in-visual-studio?view=msvc-160&viewFallbackFrom=vs-2019)

&emsp; [github cmake-example 跟着例子学](https://github.com/ttroy50/cmake-examples)

&emsp; [CGold博客](https://cgold.readthedocs.io/en/latest/overview.html)

&emsp;以后再补充


