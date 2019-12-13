---
title: 『题解』Codeforces10D LCIS
categories: 题解
tags:
    - 题解
    - LCIS
    - DP
    - Codeforces
    - 洛谷
    - C++
mathjax: true
---

### Portal:

Portal1: [Codeforces](http://codeforces.com/problemset/problem/10/D)

Portal2: [Luogu](https://www.luogu.com.cn/problem/CF10D)

### Description

*This problem differs from one which was on the online contest.*

The sequence $a_1, a_2, \cdots , a_n$ is called increasing, if $a_i < a_{i + 1}$ for $i < n$.

The sequence $s_1, s_2, \cdots , a_n$ is called the subsequence of the sequence $a_1, a_2, \cdots , a_n$, if there exist such a set of indexes $1 \le i_1 < i_2 < \cdots < i_k \le n$ that $a_{i, j} = s_j$. In other words, the sequence $s$ can be derived from the sequence $a$ by crossing out some elements.

You are given two sequences of integer numbers. You are to find their longest common increasing subsequence, i.e. an increasing sequence of maximum length that is the subsequence of both sequences.

### Input

The first line contains an integer $n (1 \le n \le 500)$ — the length of the first sequence. The second line contains $n$ space-separated integers from the range $[0, 10^9]$ — elements of the first sequence. The third line contains an integer $m (1 \le m \le 500)$ — the length of the second sequence. The fourth line contains *m* space-separated integers from the range $[0, 10^9]$ — elements of the second sequence.

### Output

In the first line output $k$ — the length of the longest common increasing subsequence. In the second line output the subsequence itself. Separate the elements with a space. If there are several solutions, output any.

### Sample Input1

```
7
2 3 1 6 5 4 6
4
1 3 5 6
```

### Sample Output1

```
3
3 5 6
```

### Sample Input2

```
5
1 2 0 2 1
3
1 0 1
```

### Sample Output2

```
2
0 1
```

### Description in Chinese

求两个序列的最长公共上升子序列。

### Solution

用$\texttt{f[i][j]}$表示$\texttt{A}$与$\texttt{B}$构成的以$b_j$为结尾的最长公共上升子序列的长度。

动态转移方程为：

$$f[i][j] = \max_{k = 0}^{j - 1}{(f[i - 1][k] + 1)}, \texttt{其中}B_k < B_j$$

而$k$这一维可以省去，在状态转移后记录最大符合要求的$f[k]$作为下一次的转移，正确性仍旧不变。

所以，最后的时间复杂度为$O(n^2)$。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

const int MAXN = 505;
int n, m, a[MAXN], b[MAXN], ans[MAXN], dp[MAXN], scheme[MAXN];
int main() {
    scanf("%d", &n);
    for (int i = 1; i <= n; i++)
        scanf("%d", &a[i]);
    scanf("%d", &m);
    for (int i = 1; i <= m; i++)
        scanf("%d", &b[i]);
    for (int i = 1; i <= n; i++) {//动态规划主要部分
        int id = 0;
        for (int j = 1; j <= m; j++) {
            if (a[i] == b[j]) {
                dp[j] = dp[id] + 1;
                scheme[j] = id;
            }
            if (a[i] > b[j] && dp[id] < dp[j]) id = j;
        }
    }
    int id = 0;
    for (int i = 1; i <= m; i++)
        if (dp[i] > dp[id]) id = i;
    printf("%d\n", dp[id]);
    int top = 0;
    while (dp[id]--) {
        ans[++top] = b[id];
        id = scheme[id];
    }
    for (int i = top; i >= 1; i--)
        printf("%d ", ans[i]);//输出方案前需要记录
    return 0;
}
```