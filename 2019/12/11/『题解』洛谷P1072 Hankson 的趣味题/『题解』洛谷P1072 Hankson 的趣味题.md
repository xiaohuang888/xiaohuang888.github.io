---
title: 『题解』洛谷P1072 Hankson 的趣味题
categorise: 题解
tags:
    - 题解
    - 数论，数学
    - NOIP
    - 洛谷
    - LibreOJ
    - C++
mathjax: true
---

### Portal

Portal1: [Luogu](https://www.luogu.com.cn/problem/P1072)

Portal2: [LibreOJ](https://loj.ac/problem/2589)

Portal3: [Vijos](https://vijos.org/p/1753)

<!-- more -->

### Description

`Hanks`博士是`BT`（`Bio-Tech`，生物技术）领域的知名专家，他的儿子名叫`Hankson`。现在，刚刚放学回家的`Hankson`正在思考一个有趣的问题。

今天在课堂上，老师讲解了如何求两个正整数$c_1$和$c_2$的最大公约数和最小公倍数。现在`Hankson`认为自己已经熟练地掌握了这些知识，他开始思考“求公约数”和“求公倍数”这类问题的一个逆问题，这个问题是这样的：已知正整数$a_0,a_1,b_0,b_1$，设某未知正整数$x$满足：

1. $x$和$a_0$的最大公约数是$a_1$；

1. $x$和$b_0$的最小公倍数是$b_1$。

`Hankson`的“逆问题”就是求出满足条件的正整数$x$。但稍加思索之后，他发现这样的$x$并不唯一，甚至可能不存在。因此他转而开始考虑如何求解满足条件的$x$的个数。请你帮助他编程求解这个问题。

### Input

第一行为一个正整数$n$，表示有$n$组输入数据。

接下来的$n$行每行一组输入数据，为四个正整数$a_0,a_1,b_0,b_1$，每两个整数之间用一个空格隔开。

输入数据保证$a_0$能被$a_1$整除，$b_1能被$b_0$整除。

### Output

共$n$行。每组输入数据的输出结果占一行，为一个整数。

对于每组数据：若不存在这样的$x$，请输出$0$；若存在这样的$x$，请输出满足条件的$x$的个数。

### Sample Input

```
2
41 1 96 288
95 1 37 1776
```

### Sample Output

```
6
2
```

### Solution

1. $x$和$a_0$的最大公约数是$a_1$；

$\quad \gcd(x, a_0) = a_1$

2. $x$和$b_0$的最小公倍数是$b_1$。

$\quad \mathrm{lcm}(x, b_0) = b_1 \\\ \Rightarrow b_1 = x \times b_0 \div \gcd(x, b_0) \\\ \Rightarrow x \times b_0 = b_1 \times \gcd(x, b_0) \\\ \Rightarrow x = \frac{b_1}{b_0} \times \gcd(x, b_0)$

我们需要证明：$\gcd(\frac{x}{a_1}, \frac{a_0}{a_1}) = 1$，不妨使用反证法。

令$\gcd(\frac{x}{a_1}, \frac{a_0}{a_1}) \ne 1$，它的值为$k$，

$\therefore \frac{x}{a_1} = k \times p, \frac{a_0}{a_1} = k \times q \\\ \therefore x = k \times p \times a_1, a_0 = k \times q \times a_1 \\\ \therefore \gcd(x, a_0) \ne k \times a_1, \text{与原命题不符} \\\ \therefore \gcd(\frac{x}{a_1}, \frac{a_0}{a_1}) = 1$

所以我们只要枚举$b_1$的因子个数。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

int T;
int main() {
    scanf("%d", &T);
    while (T--) {
        int a0, a1, b0, b1;
        scanf("%d%d%d%d", &a0, &a1, &b0, &b1);
        if (b1 % b0 || a0 % a1) {
            printf("0\n");
            continue;
        }
        int ans = 0;
        for (int i = 1; i * i <= b1; i++)
            if (b1 % i == 0) {
                if (i % a1 == 0 && (__gcd(a0 / a1, i / a1) == 1 && __gcd(b1 / b0, b1 / i) == 1)) ans++;
                if (b1 / i != i && b1 / i % a1 == 0 && (__gcd(a0 / a1, b1 / i / a1) == 1 && __gcd(b1 / b0, i) == 1)) ans++;
            }
        printf("%d\n", ans);
    }
    return 0;
}
```