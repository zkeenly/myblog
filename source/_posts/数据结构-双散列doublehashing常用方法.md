---
title: '[数据结构]双散列doublehashing常用方法'
date: 2016-07-13 10:54:56
categories: ACM/算法
tags:
comments:
---





![2016-07-13-1](https://zkeenly.com/images/2016-07-13-1.png)

![2016-07-13-2](https://zkeenly.com/images/2016-07-13-2.png)

![2016-07-13-3](https://zkeenly.com/images/2016-07-13-3.png)

![2016-07-13-4](https://zkeenly.com/images/2016-07-13-4.png)

[(X mod TableSize) +F(i)] mod TableSize

其中F(i) = i*hash2(X)

function F(i)    hash2(X) = R-(X mod R)  //R 可自己选择。此function也可自己定义。

参考文献；

[1] Weiss M A, 冯舜玺. 数据结构与算法分析——C 语言描述[J]. 2004.

[2] Weiss M A. Data Structures and Algorithm Analysis in C: For Anna University, 2/e[M]. Pearson Education India, 2002.