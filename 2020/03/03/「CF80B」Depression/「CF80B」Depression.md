---
title: 「CF80B」Depression
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

Portal1: [Codeforces](http://codeforces.com/problemset/problem/80/B)

Portal2: [Luogu](https://www.luogu.com.cn/problem/CF80B)

### Solution

首先我们可以确定分针的偏转角度为$m \times 6$（它不受时针影响）。

时针的话可以先算出自己因整小时影响的偏转角度为$n \times 30$，然后再加上分针的影响为$m \times 0.5$，即偏转角度为$n \times 30 + m \times 0.5$。

当时间超过半天（`12:00`）时，要把小时数减去$12$（也就是时针已经转了一圈，角度从$0$开始）。

输出的时候注意一下就好了。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

int n, m;
int main() {
    scanf("%d:%d", &n, &m);
    if (n >= 12) n -= 12;
    double ans1 = n * 30 + m * 0.5;//计算时针偏转角度
    int ans2 = m * 6;//计算分针偏转角度
    if ((int)ans1 == ans1) printf("%.0lf %d\n", ans1, ans2); else printf("%.1lf %d\n", ans1, ans2);//注意输出
    return 0;
}
```