---
title: 【ACM/算法】Hilbert曲线
date: 2018-11-17 20:41:25
categories: ACM/算法
tags: 
	- 算法
---
题目：
![image](https://user-images.githubusercontent.com/6647857/46865616-bc7fd580-ce50-11e8-9704-e85b531805b2.png)
![image](https://user-images.githubusercontent.com/6647857/46865622-c1448980-ce50-11e8-97ea-050ce5104503.png)
![image](https://user-images.githubusercontent.com/6647857/46865645-d9b4a400-ce50-11e8-9d04-4737ae7fd456.png)
![image](https://user-images.githubusercontent.com/6647857/46865669-e5a06600-ce50-11e8-9c3d-10fa5cf3138f.png)

解题方法：
首先po一个很直观的对hilbert的理解：
https://www.bilibili.com/video/av4201747?from=search&seid=12530994814698419444
看完这个视频基本上就对这个曲线有所了解了，解法当然也就显而易见，就是首先构建一个基础的2*2的hilbert曲线轨迹
![image](https://user-images.githubusercontent.com/6647857/46866276-4c268380-ce53-11e8-8eac-86d0327bdb34.png)
由于要求输出结果为顺序，定义一个以上轨迹顺序的矩阵base_box：
[[2 3]
 [1 4]]
然后将这个base_box 扩展到4*4的矩阵中中上面两个2*2矩阵，
对于下面的连个2*2 矩阵，左下的矩阵进行逆时针旋转，右下的矩阵进行顺时针旋转。
然后将旋转之后的数字顺序反转一下
将左上2*2矩阵+4，右上+8，右下+12
就得到了hilbert矩阵
[[ 6  7 10 11]
 [ 5  8  9 12]
 [ 4  3 14 13]
 [ 1  2 15 16]]
最终这个4*4的矩阵就做好了，下一次继续以此办法迭代将会得到8*8的矩阵。
[[22 23 26 27 38 39 42 43]
 [21 24 25 28 37 40 41 44]
 [20 19 30 29 36 35 46 45]
 [17 18 31 32 33 34 47 48]
 [16 13 12 11 54 53 52 49]
 [15 14  9 10 55 56 51 50]
 [ 2  3  8  7 58 57 62 63]
 [ 1  4  5  6 59 60 61 64]]

附上code：
[hilbert](https://github.com/zkeenly/articles/blob/master/hilbert.py)
此程序输入输出：
输入：一个数字n
输出：n个hilbert矩阵，二阶，四阶，八阶...
如需题目输入输出样式可自行调节