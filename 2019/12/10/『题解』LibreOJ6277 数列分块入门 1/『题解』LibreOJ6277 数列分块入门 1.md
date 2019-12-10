---
title: 『题解』LibreOJ6277 数列分块入门 1
categories: 题解
tags:
    - 题解
    - 分块
    - 线段树
    - LibreOJ
    - C++
---

### Portal

Portal1: [LibreOJ](https://loj.ac/problem/6277)

### Description

给出一个长为$n$的数列，以及$n$个操作，操作涉及区间加法，单点查值。

### Input

第一行输入一个数字$n$。

第二行输入$n$个数字，第$i$个数字为$a_i$，以空格隔开。

接下来输入$n$行询问，每行输入四个数字$opt$、$l$、$r$、$c$，以空格隔开。

若$\texttt{opt = 0}$，表示将位于$[l,r]$的之间的数字都加$c$。

若$\texttt{opt = 1}$，表示询问$a_i$的值（$l$和$c$忽略）。

### Output

对于每次询问，输出一行一个数字表示答案。

### Sample Input

```
4
1 2 2 3
0 1 3 1
1 0 1 0
0 1 2 2
1 0 2 0
```

### Output

```
2
5
```

### Hint

对于$100\%$的数据，$1 \le n \le 50000, -2^{31} \le others, ans \le 2^{31} - 1$。

### Solution

分块，先将序列分成$\sqrt{n}$块，区间加法时，整块左右的边角料暴力处理，整的块来更新懒标记。单点求值时，只要把自己的值与它所在的块的懒标记加起来就可以了。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

const int MAXN = 50005;
int n, a[MAXN], bl[MAXN], tag[MAXN];
int main() {
    scanf("%d", &n);
    int block = (int)sqrt(n);//总块数
    for (int i = 1; i <= n; i++) {
        scanf("%d", &a[i]);
        bl[i] = (i - 1) / block;//分块
    }
    for (int i = 1; i <= n; i++) {
        int opt, x, y, val;
        scanf("%d%d%d%d", &opt, &x, &y, &val);
        if (opt == 0) {
            if (bl[x] == bl[y]) {
                for (int i = x; i <= y; i++)
                    a[i] += val;//如果区间不包含任何块
            } else {
                for (; bl[x] == bl[x - 1]; x++)
                    a[x] += val;//处理边角料
                for (; bl[y] == bl[y + 1]; y--)
                    a[y] += val;//处理边角料
                for (int i = x; i <= y; i += block)
                    tag[bl[i]] += val;//更新整块的懒标记
            }
        } else printf("%d\n", a[y] + tag[bl[y]]);//单点求值
    }
    return 0;
}
```

### Attachment

测试数据下载：https://www.lanzous.com/i51c8of