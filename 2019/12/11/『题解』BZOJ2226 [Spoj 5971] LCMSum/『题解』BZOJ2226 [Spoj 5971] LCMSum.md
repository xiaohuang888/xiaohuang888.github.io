---
title: 『题解』BZOJ2226 [Spoj 5971] LCMSum
categories: 题解
tags:
    - 题解
    - 数论，数学
    - BZOJ
    - C++
mathjax: true
---

### Portal

Portal: [BZOJ](https://www.lydsy.com/JudgeOnline/problem.php?id=2226)

### Description

Given $n$, calculate the sum `LCM(1,n) + LCM(2,n) + .. + LCM(n,n)`, where `LCM(i,n)` denotes the Least Common Multiple of the integers $i$ and $n$.

### Input

The first line contains $T$ the number of test cases. Each of the next $T$ lines contain an integer $n$.

### Output

Output $T$ lines, one for each test case, containing the required `sum`.

### Sample Input

```
3
1
2
5
```

### Sample Output

```
1
4
55
```

### Hint

$1 \le T \le 300000$，$1 \le n \le 1000000$。

## Description in Chinese

给定正整数$N$，求`LCM(1, N) + LCM(2, N) + ... + LCM(N, N)`。

### Solution

题目中的式子可以化简为：

$$\begin{align} \sum^{n}_{i = 1}{\text{lcm}(i, n)} & = \sum^{n}_{i = 1}{\frac{i \times n}{\gcd(i, n)}} \\\ & = n \times \sum^{n}_{i = 1}{\frac{i}{\gcd(i, n)}} \\\ & = n \times \sum_{d | n}\sum_{i = 1}^{n}\frac{i}{d} \times (d == \gcd(i, n)) \\\ & = \frac{n}{d} \times \sum_{d | n}\sum^{n}_{i = 1}d == \gcd(i, n) \end{align} \\\\ \text{当}\gcd(i, n) == 1 \text{时，} \gcd(n - i, n) == 1 (i, n - i \ne 1) \\\\ \therefore \frac{n}{d} \times \sum_{d | n}\sum^{n}_{i = 1}d == \gcd(i, n) \\\\ = \sum_{i = 1}^{n}i \times (\gcd(i, n) == 1)= n \times \frac{\varphi(d)}{2}$$

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

typedef long long LL;
const int MAXN = 1000005;
int T;
LL n, cnt, ans[MAXN], phi[MAXN], prime[MAXN];
inline void calc_phi() {//计算phi函数
    phi[1] = 1;
    for (int i = 2; i <= MAXN; i++) {
        if (!phi[i]) {
            prime[++cnt] = i;
            phi[i] = i - 1;
        }
        for (int j = 1; j <= cnt; j++) {
            if (i * prime[j] >= MAXN) break;
            if (i % prime[j] == 0) {
                phi[i * prime[j]] = phi[i] * prime[j];
                break;
            } else phi[i * prime[j]] = phi[i] * (prime[j] - 1);
        }
    }
}
int main() {
    scanf("%d", &T);
    calc_phi();
    for (int i = 1; i <= MAXN; i++)
        for (int j = i; j <= MAXN; j += i)
            ans[j] += phi[(j / i)] * (j / i) + 1 >> 1;//最后推出的式子
    while (T--) {
        scanf("%lld", &n);
        printf("%lld\n", n * ans[n]);//最后主题要×n
    }
    return 0;
}
```

### Attachment

测试数据下载：https://www.lanzous.com/i59jdri