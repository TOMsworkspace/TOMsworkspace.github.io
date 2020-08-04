---
title: C语言缓冲区
comments: true
copyright: true
date: 2020-02-24 20:30:53
updated:
tags: C 缓冲区
categories: C
keywords: C 缓冲区
description: C 缓冲区 完全缓冲 行缓冲
top_img: https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/C语言中输入输出所有格式控制符/figure1.jpg
cover: https://cdn.jsdelivr.net/gh/TOMsworkspace/BlogHelper/C语言中输入输出所有格式控制符/figure1.jpg
toc: true
toc_number: true
---

# C语言缓冲区

&emsp;在编写C语言的交互式程序时，在和程序进行交互的过程中，不知不觉地用使用到了缓冲区的概念。下面来了解一下C语言的缓冲区。

## 为什么要有缓冲区

&emsp;首先，把若干字符作为一个块传输比逐个发送字符节省时间；其次，如果用户打错字符，可以直接通过键盘修改错误。当最后按下Enter键时，发送的才是正确的输入。

## 缓冲区的分类

&emsp;C语言在不同的地方根据需要使用不同的缓冲区。缓冲区分为两类：完全缓冲I/O和行缓冲I/O。完全缓冲是指的是当前缓冲区被填满时才刷新缓冲区（内容被发送至目的地），通常出现在文件输入中。缓冲区的大小取决于系统，常见的大小是512字节和4096字节。行缓冲指的是在出现换行时刷新缓冲区。键盘输入通常是行缓冲输入，所以在按下Enter键时才刷新缓冲区。

