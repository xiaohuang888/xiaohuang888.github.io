---
title: 『题解』Codeforces656E Out of Controls
categories: 题解
tags:
    - 题解
    - 最短路
    - Floyd
    - Codeforces
    - 洛谷
    - C++
mathjax: true
---

### Portal

Portal1: [Codeforces](http://codeforces.com/problemset/problem/656/E)

Portal2: [Luogu](https://www.luogu.com.cn/problem/CF656E)

<!-- more -->

### Description

You are given a complete undirected graph. For each pair of vertices you are given the length of the edge that connects them. Find the shortest paths between each pair of vertices in the graph and return the length of the longest of them.

### Input

The first line of the input contains a single integer $N (3 \le N \le 10)$.

The following $N$ lines each contain $N$ space-separated integers. jth integer in ith line aij is the length of the edge that connects vertices $i$ and $j$. $a_{ij} = a_{ji}, a_{ii} = 0, 1 \le a_{ij} \le 100$ for $i \ne j$.

### Output

Output the maximum length of the shortest path between any pair of vertices in the graph.

### Sample Input1

```
3
0 1 1
1 0 4
1 4 0
```

### Sample Output1

```
2
```

### Sample Input2

```
4
0 1 2 3
1 0 4 5
2 4 0 6
3 5 6 0
```

### Sample Output2

```
5
```

### Hint

You're running short of keywords, so you can't use some of them:

```
define
do
for
foreach
while
repeat
until
if
then
else
elif
elsif
elseif
case
switch
```

### Solution

这题的$n$最大只有$10$，求的是两点见的最大的最短路，我们直接用`Floyd`解决。

但这是一道愚人节的题，题目规定我们不能使用一些语法，我们直接可以用这样的形式解决：

```
#defin\
e ... ...
```

**注意，把`for`改为大写还是会判错。**

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>
#defin\
e rep fo\
r

using namespace std;

const int INF = 0x3f3f3f3f, MAXN = 15;
int n, map[MAXN][MAXN];
int main() {
    scanf("%d", &n);
    rep (int i = 1; i <= n; i++)
        rep (int j = 1; j <= n; j++)
            scanf("%d", &map[i][j]);
    rep (int k = 1; k <= n; k++)
        rep (int i = 1; i <= n; i++)
            rep (int j = 1; j <= n; j++)
                map[i][j] = min(map[i][j], map[i][k] + map[k][j]);//Floyd
    int ans = -INF;
    rep (int i = 1; i <= n; i++)
        rep (int j = 1; j <= n; j++)
            ans = max(ans, map[i][j]);
    printf("%d\n", ans);
}
```