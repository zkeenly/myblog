---
title: '[ACM/算法]算法硬币问题'
date: 2019-03-09 11:23:25
categories: ACM/算法
tags:
	- 算法
comments: true
---

动态规划问题之一，硬币问题。

### 问题描述

假设有 1 元，3 元，5 元的硬币若干（无限），现在需要凑出 11 元，问如何组合才能使硬币的数量最少？

输入硬币种类数量为number，需要凑出的金钱为money

接下来number列，输入所有的硬币种类money_list[]

### 问题分析

我们从凑出1元开始，依次递进，最终得到凑出11元的结果。

1. 假设res_list[i] 为凑出i元所需要的硬币数量

2. 我们生成状态转移方程：

   res_list[i] =min<sub>j=1...number</sub>[res_list[i-coin<sub>j</sub>] + 1)]，其中coin<sub>j</sub>为所有硬币种类硬币序列

   即 当前凑出i的 最优解数量是凑出 i-coin<sub>j</sub> 的最优解 +1，且+1所代表的硬币是coin<sub>j</sub>。

3. res_list[0] 必定是0，res_list[1] = min<sub>j = 1... number</sub>[res_list[1-coin<sub>j</sub>] +1]

4. 每次得到res_list[i]的值之后，都将当前的res_order[i-coin<sub>j</sub>]和coin<sub>j</sub>保存到res_order[i]中，作为硬币序列。

### 算法代码

```python
# 输入硬币种类数量和需要凑的金钱
number, money = input().split(' ')
# 金钱种类列表，存放所有类型的硬币
money_lists = []
# 输入所有硬币种类的面值
for i in range(int(number)):
    money_lists.append(int(input()))

res_list = [0 for i in range(int(money)+1)]  # 保存结果列表，每个数组都是该情况最小的种类数量
res_order = [[0 for j in range(0)] for i in range(int(money)+1)]  # 保存结果序列，其中每一个数组都保存了一个该情况的最小硬币序列
for i in range(1, int(money)+1):  # 遍历所需要凑的金额
    current_coin = -1
    min_res = -1
    for j in money_lists:  # 遍历硬币面值
        if i >= j:  # 确定不会超出界限，即当前需要的硬币总额不小于当前硬币面额
            if min_res == -1:
                min_res = res_list[i-j]+1
                current_coin = j
            elif res_list[i-j]+1 < min_res:
                min_res = res_list[i-j]+1
                current_coin = j
    res_list[i] = min_res  # 将最小的种类组合个数复制给当前res_lists
    for k in res_order[i-current_coin]:  # 保存当前硬币序列到列表order。
        # print(res_order[i-current_coin])
        # print(i)
        if k != 0:
            res_order[i].append(k)
    if current_coin != 0:
        res_order[i].append(current_coin)

# print(res_list)
print(res_order)
```

算法代码点击 [这里](https://github.com/zkeenly/articles/blob/master/CoinsProblems.py)

#### 引用：

https://www.cnblogs.com/snowInPluto/p/5992846.html