---
title: CNN中的参数量与占用内存计算
date: 2018-11-18 14:01:58
categories: 深度学习
tags:
	- 深度学习
comments: ture
---
卷积网络的参数计算，实际上就是卷积核的数量统计。
<img width="581" alt="image" src="https://user-images.githubusercontent.com/6647857/48416886-50182f00-e78c-11e8-8b17-c5d7da6a1bf0.png">
这张图很好的体现了卷积网络的过程：
假设input为三层，就需要三个卷积核对其分别卷积，之后将卷积结果累加得到一层特征图。
如果需要得到2层特征图的结果，就需要3 * （三个卷积核） 共需要六个卷积核来生成2层特征图。
每个卷积核由3 * 3个参数组成，所以上图共需要3 * 2 *（3 * 3）个参数。
input层的占用内存为3 *（5*5），output层的占用内存为2 *（3 * 3）。
于是就得到下图关于VGG16内存占用以及参数的计算结果：
<img width="586" alt="image" src="https://user-images.githubusercontent.com/6647857/48417197-1c89d480-e78d-11e8-9c3c-66712274070b.png">

引用：http://cs231n.github.io/convolutional-networks/