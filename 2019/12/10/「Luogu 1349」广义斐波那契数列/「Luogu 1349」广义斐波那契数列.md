---
title: 「Luogu 1349」广义斐波那契数列
categories: 题解
tags:
    - 题解
    - 矩阵
    - 快速幂
    - 数学，数论
    - C++
---

### Portal

Portal1: [Luogu](https://www.luogu.com.cn/problem/P1349)

### Description

广义的斐波那契数列是指形如$an=p \times a_{n-1}+q \times a_{n-2}$的数列。今给定数列的两系数$p$和$q$，以及数列的最前两项$a_1$和$a_2$，另给出两个整数$n$和$m$，试求数列的第$n$项$a_n$除以$m$的余数。

### Input

输入包含一行6个整数。依次是$p$,$q$,$a_1$,$a_2$,$n$,$m$，其中在$p$,$q$,$a_1$,$a_2$整数范围内，$n$和$m$在长整数范围内。

### Output

输出包含一行一个整数，即$a_n$除以$m$的余数。

### Sample Input

```
1 1 1 1 10 7
```

### Sample Output

```
6
```

### Hint

数列第$10$项是$55$，除以$7$的余数为$6$。

### Solution

基本斐波那契数列矩阵是$T = \begin{bmatrix} 1 & 1 \\ 1 & 0 \end{bmatrix}$；

广义斐波那契数列矩阵是$F = \begin{bmatrix} p & 1 \\ q & 0 \end{bmatrix}$。

那么要求的就是：

$$\begin{aligned} F_i & = F_{i - 1} \times T \\\\ & = \begin{bmatrix} f_{i - 1} & f_{i - 2} \\ 0 & 0 \end{bmatrix} \times \begin{bmatrix} 1 & 1 \\ 1 & 0 \end{bmatrix} \\\\ & = \begin{bmatrix} f_{i - 1} + f_{i - 2} & f_{i - 1} \\ 0 & 0 \end{bmatrix} \\\\ & = \begin{bmatrix} f_i & f_{i - 1} \\ 0 & 0 \end{bmatrix} \end{aligned}$$

然后就可以用矩阵快速幂来解决了。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

typedef long long LL;

struct Matrix {
    LL a[2][2];
    inline void clear() {//矩阵清空
        memset(a, 0, sizeof(a));
    }
    inline void init() {//单位矩阵
        memset(a, 0, sizeof(a));
        for (int i = 0; i < 2; i++)
            a[i][i] = 1;
    }
};
LL n, p, q, a1, a2, mod;
Matrix F, a, ans;
inline LL Plus(LL x, LL y) {
    x += y;
    if (x >= mod) x -= mod;
    return x;
}
inline LL power(LL x, LL y) {//快速幂
    LL ret = 0;
    while (y) {
        if (y & 1) ret = (ret + x) % mod;
        x = (x + x) % mod;
        y >>= 1;
    }
    return ret;
}
Matrix operator * (Matrix a, Matrix b) {//矩阵乘法
    Matrix ret;
    ret.clear();
    for (int i = 0; i < 2; i++)
        for (int j = 0; j < 2; j++)
            for (int k = 0; k < 2; k++)
                ret.a[i][j] = Plus(ret.a[i][j] % mod, power(a.a[i][k], b.a[k][j])% mod) % mod;
    return ret;
}
inline Matrix Matrix_Power(Matrix a, LL x) {//矩阵快速幂
    Matrix ret;
    ret.init();
    while (x) {
        if (x & 1) ret = ret * a;
        x >>= 1;
        a = a * a;
    }
    return ret;
}
int main() {
    scanf("%lld%lld%lld%lld%lld%lld", &q, &p, &a1, &a2, &n, &mod);
    F.a[0][0] = a1, F.a[0][1] = a2;
    a.a[0][0] = 0, a.a[1][0] = 1, a.a[0][1] = p; a.a[1][1] = q;
    ans = F * Matrix_Power(a, n - 2);
    printf("%lld\n", ans.a[0][1] % mod);
    return 0;
}
```