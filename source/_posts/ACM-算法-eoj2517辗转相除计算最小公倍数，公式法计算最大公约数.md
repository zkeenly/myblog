---
title: '[ACM/算法]eoj2517辗转相除计算最小公倍数，公式法计算最大公约数'
date: 2016-07-12 10:48:16
categories: ACM/算法
tags:
comments:
---

```c
#include <stdio.h>
#include <stdlib.h>
int main(int argc, char *argv[])
 {
 int n,a,b;
 scanf(“%d”,&n);
 while(n–){
 scanf(“%d %d”,&a,&b);
 printf(“%d %d\n”,GCD(a,b),a*b/GCD(a,b));
 }
 system(“pause”);
 return 0;
 }
 void
 change(int *a,int *b){
 int temp;
 temp = *a;
 *a = *b;
 *b = temp;
 }
 int
 GCD(int a,int b){
 int x;
 while(b)
 {
 if(a<b){
 change(&a, &b); //
 }
 x = a % b;
 a = b;
 b = x;
 }
 return a;
 }
 其中 a*
```

b/GCD(a,b) 为计算最小公倍数的公式。