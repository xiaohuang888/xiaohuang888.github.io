---
title: 『题解』Codeforces888B Buggy Robot
categories: 题解
tags:
    - 题解
    - 字符串
    - 模拟
    - Codeforces
    - 洛谷
    - C++
mathjax: true
---

### Portal

Portal1: [Codeforces](http://codeforces.com/problemset/problem/888/B)

Portal2: [Luogu](https://www.luogu.com.cn/problem/CF888B)

### Description

Ivan has a robot which is situated on an infinite grid. Initially the robot is standing in the starting cell $(0, 0)$. The robot can process commands. There are four types of commands it can perform:

* $\mathrm{U}$ — move from the cell $(x, y)$ to $(x, y + 1)$;

* $\mathrm{D}$ — move from $(x, y)$ to $(x, y - 1)$;

* $\mathrm{L}$ — move from $(x, y)$ to $(x - 1, y)$;

* $\mathrm{R}$ — move from $(x, y)$ to $(x + 1, y)$.

Ivan entered a sequence of $n$ commands, and the robot processed it. After this sequence the robot ended up in the starting cell $(0, 0)$, but Ivan doubts that the sequence is such that after performing it correctly the robot ends up in the same cell. He thinks that some commands were ignored by robot. To acknowledge whether the robot is severely bugged, he needs to calculate the maximum possible number of commands that were performed correctly. Help Ivan to do the calculations!

### Input

The first line contains one number $n$ — the length of sequence of commands entered by Ivan $(1  \leq n  \leq 100)$.

The second line contains the sequence itself — a string consisting of $n$ characters. Each character can be $\mathrm{U}$, $\mathrm{D}$, $\mathrm{L}$ or $\mathrm{R}$.

### Output

Print the maximum possible number of commands from the sequence the robot could perform to end up in the starting cell.

### Sample Input1

```
4
LDUR
```

### Sample Output1

```
4
```
### Sample Input2

```
5
RRRUU
```

### Sample Output2

```
0
```
### Sample Input3

```
6
LLRRRR
```

### Sample Output3

```
4
```

### Description in Chinese

给你一个字符串，让你求$2 \times (\min(\texttt{'L'的个数, 'R'的个数}) + \min(\texttt{'U'的个数，'D'的个数}))$。

### Solution

给你一个字符串，先计算出$\mathrm{L}$，$\mathrm{R}$，$\mathrm{U}$，$\mathrm{D}$的个数，最后按题目给出的表达式输出即可。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

int n;
string st;
int main() {
    scanf("%d", &n);
    cin >> st;
    int L = 0, R = 0, U = 0, D = 0;
    for (int i = 0; i < st.length(); i++)
        if (st[i] == 'L') L++; else
        if (st[i] == 'R') R++; else
        if (st[i] == 'U') U++; else
        if (st[i] == 'D') D++;//统计数量
    printf("%d\n", (min(L, R) + min(U, D)) << 1);
    return 0;
}
```