---
title: 『题解』Codeforces735D Taxes
categories: 题解
tags:
    - 题解
    - 数论，数学
    - Codeforces
    - 洛谷
    - C++
---

### Portal

Portal1: [Codeforces](http://codeforces.com/problemset/problem/735/D)

Portal2: [Luogu](https://www.luogu.com.cn/problem/CF735D)

### Description

Mr. Funt now lives in a country with a very specific tax laws. The total income of mr. Funt during this year is equal to $n (n \le 2)$ burles and the amount of tax he has to pay is calculated as the maximum divisor of $n$ (not equal to $n$, of course). For example, if $n = 6$ then Funt has to pay $3$ burles, while for $n = 25$ he needs to pay $5$ and if $n = 2$ he pays only $1$ burle.

As mr. Funt is a very opportunistic person he wants to cheat a bit. In particular, he wants to split the initial $n$ in several parts $n_1 + n_2 + \cdots + n_k = n$ (here $k$ is arbitrary, even $k = 1$ is allowed) and pay the taxes for each part separately. He can't make some part equal to $1$ because it will reveal him. So, the condition $n_i \le 2$ should hold for all i from $1$ to $k$.

Ostap Bender wonders, how many money Funt has to pay (i.e. minimal) if he chooses and optimal way to split $n$ in parts.

### Input

The first line of the input contains a single integer $n (2 \le n \le 2 \times 10^9)$ — the total year income of mr. Funt.

### Output

Print one integer — minimum possible number of burles that mr. Funt has to pay as a tax.

### Sample Input1

```
4
```

### Sample Output1

```
2
```

### Sample Input2

```
27
```

### Sample Output2

```
3
```

### Description in Chinese

某人要交税，交的税钱是收入$n$的最大因子（$\ne n$，若该数是素数则是$1$），但是现在这人为了避税，把钱拆成几份，使交税最少，输出税钱。

### Solution

1. 当$n$为素数时，显然答案为$1$；

1. 当$n$为偶数时，可以分成两个素数，也就是两个$1$，所以答案为$2$；

1. 当$n$为奇数且$n - 2$为素数时，也就是它能分成两个素数，也就是两个$1$，所以答案也是$2$；

1. 当$n$为奇数且$n - 2$不是素数时，可以分成$3$和一个偶数，偶数又可以分成$2$个素数。所以总共分成了$3$个素数，因此答案为$3$。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

int n;
inline bool check(int x) {//判断素数
    if (x < 2) return 0;
    for (int i = 2; i <= (int)sqrt(x); i++)
        if (x % i == 0) return 0;
    return 1;
}
int main() {
    scanf("%d", &n);
    if (check(n)) {
        printf("1\n");
        return 0;
    }
    if (!(n & 1) || check(n - 2)) printf("2\n"); else printf("3\n");//这里使用快速幂，应为!的优先级比&高所以必须加括号
    return 0;
}
```