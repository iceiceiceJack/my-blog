+++
title = "面试题池（三）：C/C++"
author = ["Jackie Zhang"]
date = 2019-03-06T11:29:00+08:00
lastmod = 2019-03-06T11:40:26+08:00
tags = ["interview", "cpp"]
categories = ["misc"]
draft = true
weight = 3005
+++

可以问到的C/C++相关问题。

<!--more-->


## explicit（显式）关键字 {#explicit-显式-关键字}

-   explicit 修饰构造函数时，可以防止隐式转换和复制初始化。
-   explicit 修饰转换函数时，可以防止隐式转换，但按语境转换除外。


## friend 友元类和友元函数 {#friend-友元类和友元函数}

-   能访问私有成员
-   破坏封装性
-   友元关系不可传递
-   友元关系的单向性
-   友元声明的形式及数量不受限制
