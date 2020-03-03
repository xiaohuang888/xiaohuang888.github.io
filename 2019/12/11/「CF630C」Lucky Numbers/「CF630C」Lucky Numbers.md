---
title: 「CF630C」Lucky Numbers
categories: 题解
tags:
    - 题解
    - 数学，数论
    - Codeforces
    - 洛谷
    - C++
mathjax: true
---

### Portal

Portal1: [Codeforces](http://codeforces.com/problemset/problem/630/C)

Portal2: [Luogu](https://www.luogu.com.cn/problem/CF630C)

### Description

The numbers of all offices in the new building of the Tax Office of IT City will have lucky numbers.

Lucky number is a number that consists of digits $7$ and $8$ only. Find the maximum number of offices in the new building of the Tax Office given that a door-plate can hold a number not longer than $n$ digits.

### Input

The only line of input contains one integer $n (1 \le n \le 55)$ — the maximum length of a number that a door-plate can hold.

### Output

Output one integer — the maximum number of offices, than can have unique lucky numbers not longer than $n$ digits.

### Sample Input

```
2
```

### Sample Output

```
6
```

### Solution

题目要我们构造$1 \sim n$位由$7, 8$的数的个数。我们先来找一找规律：

位数为$1$时：有$7, 8$，共$2 \times 2 ^ 0 = 2$种；

位数为$2$时：有$77, 78, 87, 88$，共$2 \times 2 ^ 1 = 4$种；

位数为$3$时：有$777, 778, 787, 788, 877, 878, 887, 888$共$2 \times 2 ^ 2 = 8$种；

$\cdots \cdots$

所以，位数是$n$的总个数是$2 \times 2 ^ {n - 1}$；

那么位数为$1 \sim n$的总个数为

$$\begin{aligned} \sum^{n}_{i = 1}{2 \times 2 ^ {i - 1}} & = 2 \times \sum^{n}_{i = 1}{2 ^ {i - 1}} \\\\ & = 2 \times (2 ^ {n} - 2) \\\\ & = 2 ^ {n + 1} - 2\end{aligned}$$

于是就解决了。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

typedef long long LL;
LL n;
inline LL power(LL x, LL y) {//求x的y次方
    LL ret = 1;
    for (LL i = 1; i <= y; i++)
        ret *= x;
    return ret;
}
int main() {
    scanf("%lld", &n);
    printf("%lld\n", power(2, n + 1) - 2);//推出来的公式
    return 0;
}
```