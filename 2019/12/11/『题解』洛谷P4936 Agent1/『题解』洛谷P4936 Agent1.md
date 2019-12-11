---
title: 『题解』洛谷P4936 Agent1
categories: 题解
tags:
    - 题解
    - 数论，数学
    - 快速幂
    - 洛谷
    - C++
mathjax: true
---

### Portal

Portal: [Luogu](https://www.luogu.com.cn/problem/P4936)

<!-- more -->

### Description

某地的`ENLIGHTENED总部`有$N$个`Agent`，每个`Agent`的能力值互不相同，现在`ENLIGHTENED行动指挥`想要派出$A, B$两队`Agent`去参加`XM大战`。但是参加大战的两个队伍要满足两个要求：

1. $A$队中能力最大的`Agent`的能力值要小于$B$队能力最弱的`Agent`的能力值。

2. $A, B$两队都要有人参战。

并不一定所有的`Agent`都要去参加`XM大战`的，心急的ENLIGHTENED行动指挥想知道有多少种安排`Agent`参加大战的方案。由于答案可能很大，所以只需要你求出答案模$(10 ^ 9 + 7)$的值就可以了。

### Input

输入仅一行，为一个整数$N$。

### Output

输出答案模$(10 ^ 9 + 7)$的值。

### Sample Input1

```
3
```

### Sample Output1

```
5
```
### Sample Input2

```
6
```

### Sample Output2

```
129
```

### Solution

根据题意，我们要取两组数，使$A$组的最大值小于$B$组的最小值，那么我们在一个有序的数列（不妨设为$1, 2, 3, \cdots , n$）里，我们可以取一个数$p$，表示分界线（不妨把它放在$A$组），$A$组的取数范围就是$p$以及$p$左边的所有数，$B$组的取数范围就是$p$右边的所有数，因此当$p = i$时$A$组取数的方案总数就是$2 ^ {i - 1}$（前面的每个数要么取，要么不取），而$B$组取数的方案数就是$2 ^ {n - i} - 1$（为什么要减$1$，因为不允许$B$队没有人参战，而我们把$p$归入$A$队，因此$A$组不会空）。我们用代数的形式表示出来：

$$\begin{align} \text{答案} & = \sum^{n}_{i = 1}{2 ^ {i - 1} \times (2 ^ {n - i} - 1)} \\\ & = \sum^{n}_{i = 1}{2 ^ {n - 1} - 2^{i - 1}} \\\ & = n \times 2 ^ {n - 1} - \sum^{n}_{i = 1}{2 ^ {i - 1}} \\\ & = n \times 2 ^ {n - 1} -\sum^{n - 1}_{i = 0}{2 ^ i} \\\ & = n \times 2 ^ {n - 1} - (2 ^ 0 + \sum^{n - 1}_{i = 1}{2 ^ i}) \\\ & = n \times 2 ^ {n - 1} - (1 + 2 ^ n - 2) \\\ & = n \times 2 ^ {n - 1} - 2^ n + 1 \\\ & = n \times 2 ^ {n - 1} - 2 \times 2 ^ {n - 1} + 1 \\\ & = (n - 2) \times 2 ^ {n - 1} + 1 \end{align}​$$

于是我们就可以直接通过快速幂直接求出答案了。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

typedef long long LL;
const int mod = 1000000007;
LL n;
inline LL power(LL x, LL p) {//快速幂
    LL ret = 1;
    for (; p; p >>= 1) {
        if (p & 1) ret = ret * x % mod;
        x = x * x % mod;
    }
    return ret;
}
int main() {
    scanf("%lld", &n);
    printf("%lld\n", ((n - 2) % mod * power(2, n - 1) % mod + 1) % mod);//公式
    return 0;
}
```