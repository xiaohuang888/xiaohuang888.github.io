---
title: 『题解』LibreOJ6278 数列分块入门 2
categories: 题解
tags:
    - 题解
    - 分块
    - 线段树
    - LibreOJ
    - C++
mathjax: true
---

### Portal

Portal1: [LibreOJ](https://loj.ac/problem/6278/)

<!-- more -->

### Description

给出一个长为$n$的数列，以及$n$个操作，操作涉及区间加法，询问区间内小于某个值$n$的元素个数。

### Input

第一行输入一个数字$n$。

第二行输入$n$个数字，第$i$个数字为$a_i$，以空格隔开。

接下来输入$n$行询问，每行输入四个数字$\mathrm{opt}$、$l$、$r$、$c$，以空格隔开。

若$\mathrm{opt} = 0$，表示将位于$[l, r]$的之间的数字都加$c$。

若$\mathrm{opt} = 1$，表示询问$[l, r]$中，小于$c ^ 2$的数字的个数。

### Output

对于每次询问，输出一行一个数字表示答案。

### Sample Input

```
4
1 2 2 3
0 1 3 1
1 1 3 2
1 1 4 1
1 2 3 2
```

### Sample Output

```
3
0
2
```

### Hint

对于$100\%$的数据，$1 \le n \le 50000, -2^{31} \le \mathrm{others}$、$\mathrm{ans} \le 2^{31}-1$。

### Solution

首先将数组分块，然后求出每一个块的最大以及最小值，修改是暴力修改边角料，整块的修改块的懒标记。

查询时，边角料还是暴力处理，对于整块：

1. 如果这个块的最大值小于目标值，那么答案增加块的大小，也就是块里的每个元素都符合要求；

1. 如果这个块的最小值大于目标值，那么答案不变，也就是块里每个元素都不符合要求；

1. 否则暴力查询。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

const int INF = 0x3f3f3f3f, MAXN = 50005;
int n, block, a[MAXN], bl[MAXN], Max[MAXN], Min[MAXN], tag[MAXN];
inline void update(int x, int y, int val) {
    for (int i = x; i <= min(bl[x] * block, y); i++) {
        a[i] += val;
        Max[bl[i]] = max(Max[bl[i]], a[i] + tag[bl[i]]);
        Min[bl[i]] = min(Min[bl[i]], a[i] + tag[bl[i]]);
    }
    if (bl[x] != bl[y]) {
        for (int i = (bl[y] - 1) * block + 1; i <= y; i++) {
            a[i] += val;
            Max[bl[i]] = max(Max[bl[i]], a[i] + tag[bl[i]]);
            Min[bl[i]] = min(Min[bl[i]], a[i] + tag[bl[i]]);
        }
    }//处理边角料，并更新修改后每个块的最大值和最小值
    for (int i = bl[x] + 1; i <= bl[y] - 1; i++)
        tag[i] += val, Max[i] += val, Min[i] += val;//整块修改
}
inline int query(int x, int y, int val) {
    int ret = 0;
    for (int i = x; i <= min(bl[x] * block, y); i++)
        if (a[i] + tag[bl[x]] < val) ret++;
    if (bl[x] != bl[y]) {
        for (int i = (bl[y] - 1) * block + 1; i <= y; i++)
            if (a[i] + tag[bl[y]] < val) ret++;
    }//处理边角料
    for (int i = bl[x] + 1; i <= bl[y] - 1; i++) {
        if (Max[i] < val) ret += block; else//如果这个块的最大值小于目标值，那么答案增加块的大小
        if (Min[i] >= val) continue; else {//如果这个块的最小值大于目标值，那么答案不变
            for (int j = (i - 1) * block + 1; j <= i * block; j++)
                if (a[j] + tag[i] < val) ret++;//否则暴力查询
        }
    }//整块的操作
    return ret;
}
int main() {
    scanf("%d", &n);
    block = (int)sqrt(n);
    for (int i = 1; i <= (n - 1) / block + 1; i++) {//初始化最大值和最小值
        Max[i] = -INF;
        Min[i] = INF;
    }
    for (int i = 1; i <= n; i++)
        bl[i] = (i - 1) / block + 1;//计算每个位置所属块的编号
    for (int i = 1; i <= n; i++) {
        scanf("%d", &a[i]);
        Max[bl[i]] = max(Max[bl[i]], a[i]);
        Min[bl[i]] = min(Min[bl[i]], a[i]);//找出每个块中的最大值和最小值
    }
    for (int i = 1; i <= n; i++) {
        int opt, x, y, val;
        scanf("%d%d%d%d", &opt, &x, &y, &val);
        if (opt == 0) update(x, y, val); else printf("%d\n", query(x, y, val * val));
    }
    return 0;
}
```

### Attachment

测试数据下载：https://www.lanzous.com/i575irc