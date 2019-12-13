---
title: 『题解』Codeforces121A Lucky Sum
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

Portal1: [Codeforces](http://codeforces.com/problemset/problem/121/A)

Portal2: [Luogu](https://www.luogu.com.cn/problem/CF121A)

### Description

**Petya loves lucky numbers. Everybody knows that lucky numbers are positive integers whose decimal representation contains only the lucky digits $4$ and $7$. For example, numbers $47$, $744$, $4$ are lucky and $5$, $17$, $467$ are not.**

Let $next(x)$ be the minimum lucky number which is larger than or equals $x$. Petya is interested what is the value of the expression $next(l) + next(l + 1) + \cdots + next(r - 1) + next(r)$. Help him solve this problem.

### Input

The single line contains two integers $l$ and $r (1 \le l \le r \le 10^9)$ - the left and right interval limits.

### Output

In the single line print the only number - the sum $next(l) + next(l + 1) + ... + next(r - 1) + next(r)$.

Please do not use the `%lld` specificator to read or write 64-bit integers in `C++`. It is preferred to use the `cin`, `cout` streams or the `%I64d` specificator.

### Sample Input1

```
2 7
```

### Sample Output1

```
33
```

### Sample Input2

```
7 7
```

### Sample Output2

```
7
```

### Sample Explain

In the first sample: $next(2) + next(3) + next(4) + next(5) + next(6) + next(7) = 4 + 4 + 4 + 7 + 7 + 7 = 33$

In the second sample: $next(7) = 7$

### Description in Chinese

`Petya`喜欢`Lucky Number`。仅含有数字$4$和$7$的数字是一个`Lucky Number`。

规定$next(x)$等于最小的大于等于$x$的`Lucky Number`。现在Petya想知道$next(l) + next(l + 1) + \cdots + next(r - 1) + next(r)$

### Solution

我们可以先预处理出所有的`Lucky Number`。

在询问时，我们可以采用前缀和的思想，题目中的询问可转换为：

$$\sum^{r}_{i = l} \mathrm{next}(i) \\\ = \sum^{r}_{i = 1}{\mathrm{next}(i)}- \sum^{l - 1}_{i = 1}{\mathrm{next}(i)}$$

当我们在询问$\sum^{x}_{i = 1}{\mathrm{next}(i)}$是，如果一个个暴力枚举一定会超时。所以我们可以把连续的一段（第一个大于等于$x$的值相同的数）一起加起来。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

typedef long long LL;
const int MAXN = 2005;
int l, r;
LL f[MAXN];
inline void prepare() {//预处理Lucky Number的值
    f[1] = 4; f[2] = 7;
    int cnt = 2;
    for (int i = 1; i <= 512; i++) {
        f[++cnt] = f[i] * 10 + 4;
        f[++cnt] = f[i] * 10 + 7;
    }
}
inline LL calc(int x) {//计算前n个next(i)的值
    LL ret = 0;
    for (int i = 1; i <= 2000; i++) {
        if (f[i] >= x) {//如果超出了x（也就是累加询问的上界），就加完退出
            ret += f[i] * (x - f[i - 1]);//询问上界的值与最大小于询问上界的值的差，也就是有多少个next(i)要一起累加
            return ret;
        } else ret += f[i] * (f[i] - f[i - 1]);//否则就一段一段加，增加的就是两个相邻的Lucky Number的差
    }
    return ret;
}
int main() {
    scanf("%d%d", &l, &r);
    prepare();
    printf("%lld\n", calc(r) - calc(l - 1));
    return 0;
}
```