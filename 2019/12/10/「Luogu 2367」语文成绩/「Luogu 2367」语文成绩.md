---
title: 「Luogu 2367」语文成绩
categories: 题解
tags:
    - 题解
    - 差分
    - 线段树
    - 分块
    - C++
---

### Portal

Portal1: [Luogu](https://www.luogu.com.cn/problem/P2367)

### Description

语文老师总是写错成绩，所以当她修改成绩的时候，总是累得不行。她总是要一遍遍地给某些同学增加分数，又要注意最低分是多少。你能帮帮她吗？

### Input

第一行有两个整数$n$，$p$，代表学生数与增加分数的次数。

第二行有$n$个数，$a_1 \sim a_n$，代表各个学生的初始成绩。

接下来$p$行，每行有三个数，$x$，$y$，$z$，代表给第$x$个到第$y$个学生每人增加$z$分。

### Output

输出仅一行，代表更改分数后，全班的最低分。

### Sample Input

```
3 2
1 1 1
1 2 1
2 3 1
```

### Sample Output

```
2
```

### Hint

对于$40\%$的数据，有$n \le 1000$；

对于$60\%$的数据，有$n \le 10000$；

对于$80\%$的数据，有$n \le 100000$；

对于$100\%$的数据，有$n \le 5000000，p \le n$，学生初始成绩$\le 100$，$z \le 100$。

### Solution

这题介绍一种$O(n)$的算法。

可以用差分数组来做，在修改的时候，第一个数加$x$，第$n+1$个数减$x$，类似于一个懒标记，查询的时候累加一下，再加上本身就是这个数的最终值。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

const int INF = 0x3f3f3f3f, MAXN = 5000005;
int n, q, l, r, val, a[MAXN], diff[MAXN];
int main() {
    scanf("%d%d", &n, &q);
    for (int i = 1; i <= n; i++)
        scanf("%d", &a[i]);
    for (int i = 1; i <= q; i++) {
        scanf("%d%d%d", &l, &r, &val);
        diff[l] += val; diff[r + 1] -= val;//差分数组
    }
    int Min = INF, tmp = 0;
    for (int i = 1; i <= n; i++) {
        tmp += diff[i];//查询这一个位置的修改值
        if (a[i] + tmp < Min) Min = a[i] + tmp;
    }
    printf("%d\n", Min);
    return 0;
}
```