---
title: '[数据结构]线性时间打印l链表中p链表元素位置的元素'
date: 2016-06-16 12:57:51
categories: ACM/算法
tags: 
	- 算法
comments: true
---

# 线性时间打印序列L中P位置的元素

P也是序列，采用方法为先将P位置序列排序，然后遍历L序列，设置计数器。

当计数器数字与P位置序列中元素相同，则打印L当前元素数据。

```c
void
PrintLots(List L,List P){
// lsort(L);
lsort(P);
Position N = P->Next;
int count = 1;

while(L->Next !=NULL && P->Next !=NULL){//线性解决方法
if(P->Next->Element == count++){
printf(“%d”,L->Next->Element);
P = P->Next;
}
L = L->Next;
}
//
//
// while(N !=NULL){ //O（N^2）解决
// lprintnumber(L,N->Element);
// N = N->Next;
// }
}
```

参考文献：

[1] 蔡子经. 施伯乐[J]. 数据结构教程, 1994.

[2]蔡子经. 施伯乐数据结构教程上海: 复旦大学出版社[J]. I999.