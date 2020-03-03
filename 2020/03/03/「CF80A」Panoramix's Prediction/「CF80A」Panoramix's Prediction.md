---
title: 「CF80A」Panoramix's Prediction
categories: 题解
tags:
    - 题解
    - 数学，数论
    - Codefoces
    - 洛谷
    - C++
mathjax: true
---

### Portal

Portal1: [Codeforces](http://codeforces.com/problemset/problem/80/A)

Portal: [Luogu](https://www.luogu.com.cn/problem/CF80A)

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

int n, m, pri[15] = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47};
int main() {
    scanf("%d%d", &n, &m);
    int x = -2, y = -2;
    for (int i = 0; i < 15; i++) {
        if (n == pri[i]) x = i;
        if (m == pri[i]) y = i;
    }
    if (fabs(x - y) == 1) printf("YES\n"); else printf("NO\n");
    return 0;
}
```