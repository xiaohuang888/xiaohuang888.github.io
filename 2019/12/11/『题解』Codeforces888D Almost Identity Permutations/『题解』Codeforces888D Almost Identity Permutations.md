---
title: 『题解』Codeforces888D Almost Identity Permutations
categories: 题解
tags:
    - 题解
    - 数论，数学
    - Codeforces
    - 洛谷
    - C++
mathjax: true
---

### Portal

Portal1: [Codeforces](https://codeforces.com/problemset/problem/888/D)

Portal2: [Luogu](https://www.luogu.com.cn/problem/CF888D)

<!-- more -->

### Description

A permutation $p$ of size $n$ is an array such that every integer from $1$ to $n$ occurs exactly once in this array.

Let's call a permutation an almost identity permutation iff there exist at least $n - k$ indices $i (1 \le i \le n)$ such that $p_i = i$.

Your task is to count the number of almost identity permutations for given numbers n and k.

### Input

The first line contains two integers $n$ and $k (4 \le n \le 1000, 1 \le k \le 4)$.

### Output

Print the number of almost identity permutations for given $n$ and $k$.

### Sample Input1

```
4 1
```

### Sample Output1

```
1
```

### Sample Input2

```
4 2
```

### Sample Output2

```
7
```

### Sample Input3

```
5 3
```

### Sample Output3

```
31
```

### Sample Input4

```
5 4
```

### Sample Output4

```
76
```

### Description in Chinese

给出$n$和$k$计算满足至少有$n - k$个位置的值$a_i = i$的$1 \sim n$的全排列的个数。

### Solution

观察题目，发现$k$的范围是$1 \sim 4$，我们不妨来分类讨论。

1. 当$k = 1$时，有$n - 1$个位置的数是固定的，也就是有$1$个位置是自由的，呵呵，因为要填$1 \sim n$，发现填的数也只有一种。

2. 当$k = 2$时，有$n - 2$个位置是固定的，还有$2$个位置是可以自由填数的，选位置的方案有$\binom{n}{2}$，我们先计算有$n - 2$个位置固定且仅有$n - 2$个位置固定的情况，每一个选位置的方案可以有$(2 - 1) \times  = 1$种放法，因为每一个位置不能填自己的编号。

```
 id: 1 2
num: 2 1
```

所以一共有$\binom{n}{2} \times 1$种，然后再加上$k = 1$的情况，即答案为$\binom{n}{2} \times 1 + 1 =\binom{n}{2} + 1$。

1. 当$k = 3$时，同情况$2$，答案为$\binom{n}{3} \times 2 + \binom{n}{2} \times 1 + 1 = 2 \times \binom{n}{3} + \binom{n}{2} + 1$。

2. 当$k = 4$时，同情况$2$，答案为$\binom{n}{4} \times 9 + \binom{n}{3} \times 2 + \binom{n}{2} \times 1 + 1 = 9 \times \binom{n}{4} + 2 \times \binom{n}{3} + \binom{n}{2} + 1$。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

typedef long long LL;
int n, k;
inline LL Combination(int x, int y) {//求组合数
    LL ret = 1;
    for (int i = x; i >= x - y + 1; i--)
        ret *= i;
    for (int i = 1; i <= y; i++)
        ret /= i;
    return ret;
}
int main() {
    scanf("%d%d", &n, &k);
    if (k == 1) printf("1\n"); else
    if (k == 2) printf("%lld\n", Combination(n, 2) + 1); else
    if (k == 3) printf("%lld\n", Combination(n, 2) + Combination(n, 3) * 2 + 1); else
    if (k == 4) printf("%lld\n", Combination(n, 2) + Combination(n, 3) * 2 + Combination(n, 4) * 9 + 1);//分类讨论
    return 0;
}
```