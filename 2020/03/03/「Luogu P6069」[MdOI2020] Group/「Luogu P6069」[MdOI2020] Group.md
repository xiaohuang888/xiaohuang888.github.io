---
title: 「Luogu P6069」[MdOI2020] Group
categories: 题解
tags:
    - 题解
    - 数学，数论
    - 洛谷
    - C++
mathjax: true
---

### Portal

Portal1: [Luogu](https://www.luogu.com.cn/problem/P6069)

### Solution

我们先将题目的式子简化一下：

$$\begin{aligned}S&=\frac{1}{n} \sum_{i=1}^n(a_i-p)^2\\S&=\frac{1}{n} \sum_{i=1}^n(a_i^2-2a_ip+p^2)\\S&=\frac{1}{n} \sum_{i=1}^n\left(a_i^2-2a_i\times\frac{sum1}{n}+\left(\frac{sum1}{n}\right)^2\right)\\nS&=sum2+\frac{sum1^2}{n}-2\times\frac{sum1^2}{n}\\nS&=sum2-\frac{sum1^2}{n}\end{aligned}$$

不难发现，要先排序，然后选取**一段连续区间的方差**一定比**选取同样个数的不连续区间的方差**小。所以我们可以二分答案（区间的长度），然后连续取每一段区间逐一判断就好了，时间复杂度为$\mathbb{O(n \log n)}$。

修改的数不用管，因为你可以修改成任意一个数。

### Code

```cpp
#include<bits/stdc++.h>
#define int __int128
//不开int128会炸
using namespace std;

typedef long long LL;
const int MAXN = 200005;
int n, m, a[MAXN], sum1[MAXN], sum2[MAXN];
LL ans;
inline int read() {//int128必须快读
    char ch = getchar();
    int x = 0, f = 1;
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while ('0' <= ch && ch <= '9') {
        x = (x << 1) + (x << 3) + ch - '0';
        ch = getchar();
    }
    return x * f;
}
inline void prepare() {
    sort(a + 1, a + n + 1);
    for (LL i = 1; i <= n; i++) {
        sum1[i] = sum1[i - 1] + a[i];
        sum2[i] = sum2[i - 1] + a[i] * a[i];
    }
}
inline bool check(LL x) {
    for (LL L = 1, R = L + x - 1; R <= n; L++, R++)
        if ((sum2[R] - sum2[L - 1]) * x - (sum1[R] - sum1[L - 1]) * (sum1[R] - sum1[L - 1]) <= x * m) return 1;
    return 0;
}
signed main() {
    n = read(), m = read();
    for (LL i = 1; i <= n; i++)
        a[i] = read();
    prepare();
    LL l = 1, r = n;
    while (l <= r) {
        LL mid = l + r >> 1;
        if (check(mid)) l = mid + 1, ans = n - mid; else r = mid - 1;
    }
    printf("%lld\n", ans);
    return 0;
}
```