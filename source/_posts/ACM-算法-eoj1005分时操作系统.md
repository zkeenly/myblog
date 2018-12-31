---
title: '[ACM/算法]eoj1005分时操作系统'
date: 2016-06-30 12:15:44
categories: ACM/算法
tags: 
	- 算法
comments: true
---

**Description** 

ACM（Advanced Computer Machine）是一种新出的计算机硬件系统，它正面临上市，但是缺少一种操作系统来配合它独特的硬件。所以，公司决为它开发一种新的操作系统。在这个操 作系统中，所有的进程都用PID来表示，它是一个正整数，每个程序都有唯一的一个PID来区分。因为它是一个分时操作系统，所以每个程序都有一定的时间， 所以系统需要写一个程序执行的序列。你作为这个开发小组一员，将负责写一个程序来生成这个序列。
ACM的一条指令Register是双字节指令，用来注册一个程序的运行，格式是这样的:
Register PID TIME

PID是程序的PID号，TIME这个PID所对应的程序运行的时间间隔(单位为MS)。PID,TIME是正整数。

另一条指令是EndRegister，用来表示Register指令的结束。
最后是指令Run，格式如下：
Run NUM
NUM生成指令的长度。

例如:
Register 2004 200
Register 2005 300
EndRegister
Run 5

那么程序执行的序列是:
2004
2005
2004
2004
2005

**Input** 

第一行有一个正整数n，表示有几个测试数据。
每一个测试数据包含一组指令（Register,EndRegister,Run），对于每一组数据，0<PID<=2^16，0< TIME<=1000，0<NUM<=10000。你可以假定没有超过1000条的Register指令，并且只有一条 EndRegister和Run指令。
如果有几条指令同时发生，那么按他们的PID大小，从小到大的输出。

**Output** 

对于每一组测试数据，第一行输出是第几个数据”test case n:”，之后的NUM行输出生成的指令。两组数据之间空一行。

**Sample Input** 

2
Register 2004 200
Register 2005 300
EndRegister
Run 5
Register 2004 100
Register 2005 200
EndRegister
Run 6

**Sample Output** 

test case 1:
2004
2005
2004
2004
2005

test case 2:
2004
2004
2005
2004
2004
2005

思路上还是比较清楚的
1、建立结构体包括PID 和TIME
2、录入数据
3、控制数据输出

#### 在第二步骤时遇到一些小麻烦，其实本题只需要验证EndResigter就可以了。不需要管是不是输入的是Resigter

```c
while(1){
    scanf("%s",&flag);
	if(strcmp(flag,"EndRegister") ==0) break;
	else{
	scanf("%d %d",&temp[count].PID,&temp[count].TIME);
	count++;
    }
}
```

在C语言中直接用scanf是最直接有效的，起初还打算用sscanf或者gets()，发现错误越陷越深。

#### 第三步骤是重点

```c
int last[10000];
for(i = 0;i<count;i++){
last[i] = 1;
}
printf("test case %d:\n",N);

for(i = 0;i<run; i++){
int min = 99999999;
int j = 0;
int k = 0;
for(j = 0;j<count;j++){   //找到最小的执行时间点。
if(last[j]*temp[j].TIME < min || last[j]*temp[j].TIME == min &&temp[j].PID <temp[k].PID){
min = last[j]*temp[j].TIME;
k = j; //j 之后自增;
}
}
last[k]++;
printf("%d\n",temp[k].PID);

}
```

需要用到last表记录每次运行一个进程是寻找最优进程的权值。就是说每次运行过一次进程之后last会自增。

last初始值为1，即该进程j开始下次运行总共所需时间间隔temp[j].TIME * last[j] 。 那么这个时间最小的进程就是即将可以运行的进程。

我们通过以下图表可以看到运行过程：

![BaiduShurufa_2016-6-30_19-1-12](https://zkeenly.com/images/2016-6-30-1.png)

通过此图可以看到进程执行过程的时间其实是忽略不计的，主要时间消耗在时间间隔。并且每一个程序的时间间隔都是相互独立的，并没有互相影响。即计算机每运行200ms就可以有一次执行PID 2004的机会，每运行300ms就有一次执行PID 2005的机会。 只有当600ms时即可以执行PID 2004 又可以PID 2005的时候才会选择PID 较小的优先执行。

通过以上分析，我们建立last数据记录每一个进程执行次数，通过计算执行次数*每次执行所需的时间 可以得出次进程下一次执行会在那个时间点，那么只需要找到最小的一个执行时间点 ，执行此进程即可。

附源程序：

```c
#include <stdio.h>
#include <stdlib.h>
/*
解题思路：
*/
typedef struct PROCESS{
	int PID;
	int TIME;
}PROCESS;


void init(int N){
	PROCESS temp[10000];
	char flag[100];
	int count = 0;
	int run;
	int i;
	while(1){
        scanf("%s",&flag);
		if(strcmp(flag,"EndRegister") ==0) break;
		else{
			scanf("%d %d",&temp[count].PID,&temp[count].TIME);
			count++;
		}
	}
	scanf("%s %d",&flag,&run);
	int last[10000];
	for(i = 0;i<count;i++){
		last[i] = 1;
	}
	printf("test case %d:\n",N);
	
	for(i = 0;i<run; i++){
        int min = 99999999;
		int j = 0;
		int k = 0;
		for(j = 0;j<count;j++){
			if(last[j]*temp[j].TIME < min || last[j]*temp[j].TIME == min &&temp[j].PID <temp[k].PID){
				min = last[j]*temp[j].TIME;
				k = j; //j 之后自增;
			}
		}
		last[k]++;
		printf("%d\n",temp[k].PID);

	}
	
	
}

int main(int argc, char *argv[])
{
	int N;
	int i;
	scanf("%d",&N);
	for(i = 1;i<=N;i++){
		init(i);
	}
	system("pause");
	return 0;
}

```

