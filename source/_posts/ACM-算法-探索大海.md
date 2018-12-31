---
title: '[ACM/算法]探索大海'
date: 2018-11-17 21:45:25
categories: ACM/算法
tags: 
	- 算法
comments: true
---
题目：
![image](https://user-images.githubusercontent.com/6647857/46867553-fa342c80-ce57-11e8-864c-bc67a02c0e0c.png)

解决方法：
这道题一眼看去可能是深度/广度优先遍历，这样做时间复杂度会比较高，也可以采用并查集的思想，具体是：
先把所有的大海的点当作坐标存入list-position_array中，
然后将我存入classify_array，
对这个position_array 进行遍历，凡是与calssify_array中元素距离等于1的，都移除存入classify_array中。当所有的距离为1的点都遍历完全之后，classify_array 的元素个数就是所能够达到的面积

这个程序其实还适用于对于目标聚类的算法（可以说算是KNN算法的实现）如下图所示：
![image](https://user-images.githubusercontent.com/6647857/46905245-15b83980-cf23-11e8-9b60-46071558f348.png)
对以上目标聚类
下面代码中存入classify_array的时候加入了类别classify_number 元素，如此当一个类别的元素分类完毕之后classify_number +1,继续计算下一个分类。最终会得到带有类别标签的list集合。

classify_array.append((position_array[0], classify_number))
[探索大海](https://github.com/zkeenly/articles/blob/master/%E6%8E%A2%E7%B4%A2%E5%A4%A7%E6%B5%B7.py)