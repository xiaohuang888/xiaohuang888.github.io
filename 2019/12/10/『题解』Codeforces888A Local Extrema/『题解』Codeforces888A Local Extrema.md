---
title: 『题解』Codeforces888A Local Extrema
categories: 题解
tags:
    - 题解
    - 枚举
    - 模拟
    - Codeforces
    - 洛谷
    - C++
---

### Portal

Portal1: [Codeforces](http://codeforces.com/problemset/problem/888/A)

Portal2: [Luogu](https://www.luogu.com.cn/problem/CF888A)

### Description

You are given an array $a$. Some element of this array $a_i$ is a local minimum iff it is strictly less than both of its neighbours (that is, $a_i < a_i - 1$ and $a_i < a_i + 1$). Also the element can be called local maximum iff it is strictly greater than its neighbours (that is, $a_i > a_i - 1$ and $a_i > a_i + 1$). Since a1 and an have only one neighbour each, they are neither local minima nor local maxima.

An element is called a *local extremum* iff it is either local maximum or local minimum. Your task is to calculate the number of local extrema in the given array.

### Input

The first line contains one integer $n (1 \le n \le 1000)$ — the number of elements in array $a$.

The second line contains $n$ integers $a1, a2,\cdots , a_n (1 \le a_i \le 1000)$ — the elements of array $a$.

### Output

Print the number of local extrema in the given array.

### Sample Input1

```
3
1 2 3
```

### Sample Output1

```
0
```

### Sample Input2

```
4
1 5 2 5
```

### Sample Output3

```
2
```

### Solution

题目怎么说我们怎么做就可以了，循环直接从$2 \to n - 1$进行枚举。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

const int MAXN = 1005;
int n, a[MAXN];
int main() {
    scanf("%d", &n);
    for (int i = 1; i <= n; i++)
        scanf("%d", &a[i]);
    int ans = 0;
    for (int i = 2; i < n; i++)
        if (a[i] > a[i - 1] && a[i] > a[i + 1] || a[i] < a[i - 1] && a[i] < a[i + 1]) ans++;//按题目模拟
    printf("%d\n", ans);
    return 0;
}
```