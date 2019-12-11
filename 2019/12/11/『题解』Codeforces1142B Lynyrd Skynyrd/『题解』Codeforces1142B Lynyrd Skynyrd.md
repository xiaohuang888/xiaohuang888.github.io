---
title: 『题解』Codeforces1142B Lynyrd Skynyrd
categories: 题解
tags:
    - 题解
    - 倍增
    - ST表
    - Codeforces
    - 洛谷
    - C++
mathjax: true
---

### Portal

Portal1: [Codeforces](http://codeforces.com/problemset/problem/1142/B)

Portal2: [Luogu](https://www.luogu.com.cn/problem/CF1142B)

<!-- more -->

### Description

Recently Lynyrd and Skynyrd went to a shop where Lynyrd bought a permutation $p$ of length $n$, and Skynyrd bought an array $a$ of length $m$, consisting of integers from $1$ to $n$.

Lynyrd and Skynyrd became bored, so they asked you $q$ queries, each of which has the following form: "does the subsegment of $a$ from the $l$-th to the $r$-th positions, inclusive, have a subsequence that is a cyclic shift of $p$?" Please answer the queries.

A permutation of length $n$ is a sequence of $n$ integers such that each integer from $1$ to $n$ appears exactly once in it.

A cyclic shift of a permutation $(p_1, p_2, \ldots, p_n)$ is a permutation $(p_i, p_{i + 1}, \ldots, p_{n}, p_1, p_2, \ldots, p_{i - 1})$ for some $i$ from $1$ to $n$. For example, a permutation $(2, 1, 3)$ has three distinct cyclic shifts: $(2, 1, 3)$, $(1, 3, 2)$, $(3, 2, 1)$.

A subsequence of a subsegment of array $a$ from the $l$-th to the $r$-th positions, inclusive, is a sequence $a_{i_1}, a_{i_2}, \ldots, a_{i_k}$ for some $i_1, i_2, \ldots, i_k$ such that $l \leq i_1 < i_2 < \ldots < i_k \leq r$.

### Input

The first line contains three integers $n$, $m$, $q$ ($1 \le n, m, q \le 2 \cdot 10^5$) — the length of the permutation $p$, the length of the array $a$ and the number of queries.

The next line contains $n$ integers from $1$ to $n$, where the $i$-th of them is the $i$-th element of the permutation. Each integer from $1$ to $n$ appears exactly once.

The next line contains $m$ integers from $1$ to $n$, the $i$-th of them is the $i$-th element of the array $a$.

The next $q$ lines describe queries. The $i$-th of these lines contains two integers $l_i$ and $r_i$ ($1 \le l_i \le r_i \le m$), meaning that the $i$-th query is about the subsegment of the array from the $l_i$-th to the $r_i$-th positions, inclusive.

### Output

Print a single string of length $q$, consisting of $0$ and $1$, the digit on the $i$-th positions should be $1$, if the subsegment of array $a$ from the $l_i$-th to the $r_i$-th positions, inclusive, contains a subsequence that is a cyclic shift of $p$, and $0$ otherwise.

### Sample Input1

```
3 6 3
2 1 3
1 2 3 1 2 3
1 5
2 6
3 5
```

### Sample Output1

```
110
```

### Sample Input2

```
2 4 3
2 1
1 1 2 2
1 2
2 3
3 4
```

### Sample Output2

```
010
```

### Hint

In the first example the segment from the $1$-st to the $5$-th positions is $1, 2, 3, 1, 2$. There is a subsequence $1, 3, 2$ that is a cyclic shift of the permutation. The subsegment from the $2$-nd to the $6$-th positions also contains a subsequence $2, 1, 3$ that is equal to the permutation. The subsegment from the $3$-rd to the $5$-th positions is $3, 1, 2$, there is only one subsequence of length $3$ ($3, 1, 2$), but it is not a cyclic shift of the permutation.

In the second example the possible cyclic shifts are $1, 2$ and $2, 1$. The subsegment from the $1$-st to the $2$-nd positions is $1, 1$, its subsequences are not cyclic shifts of the permutation. The subsegment from the $2$-nd to the $3$-rd positions is $1, 2$, it coincides with the permutation. The subsegment from the $3$ to the $4$ positions is $2, 2$, its subsequences are not cyclic shifts of the permutation.

### Solution

我们可以先预处理出$a_i$在$p$序列中的前一个数为$\mathrm{last}_i$。如果它能构成一个合法的循环序列，就代表它能够向前位移$n - 1$次$\mathrm{last}$。所以我们可以用倍增来解决。我们取一个最大的合法循环序列的头表示为$\mathrm{b}_i$，那么最后的条件就是：

$$\max ^ {r} _ {i = l}{\mathrm{b}_i} \ge l$$

满足就输出$1$，否则输出$0$。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

const int MAXN = 1000005, MAXM = 30;
int n, m, q, l, r, a[MAXN], b[MAXN], p[MAXN], last[MAXN], pos[MAXN], st[MAXN][MAXM];
inline int calc_step(int x) {
    int s = 0;
    for (int i = 25; i >= 0; i--)
        if (s + (1 << i) < n) {
            x = st[x][i];
            s += 1 << i;
        }
    return x;
}
inline int query(int l, int r) {
    int x = (int)log2(r - l + 1);
    return max(st[l][x], st[r - (1 << x) + 1][x]);//询问ST表
}
int main() {
    scanf("%d%d%d", &n, &m, &q);
    for (int i = 1; i <= n; i++) {
        scanf("%d", &p[i]);
        pos[p[i]] = i;
    }
    for (int i = 1; i <= m; i++) {
        scanf("%d", &a[i]);
        if (pos[a[i]] == 1) st[i][0] = last[p[n]]; else st[i][0] = last[p[pos[a[i]] - 1]];
        last[a[i]] = i;
    }
    for (int j = 1; j <= 25; j++)
        for (int i = 1; i <= m; i++)
            st[i][j] = st[st[i][j - 1]][j - 1];
    for (int i = 1; i <= m; i++)
        b[i] = calc_step(i);
    memset(st, 0, sizeof(st));
    for (int i = 1; i <= m; i++)
        st[i][0] = b[i];
    for (int j = 1; j <= (int)log2(m); j++)
        for (int i = 1; i <= m - (1 << j) + 1; i++)
            st[i][j] = max(st[i][j - 1], st[i + (1 << j - 1)][j - 1]);//ST表
    for (int i = 1; i <= q; i++) {
        scanf("%d%d", &l, &r);
        if (query(l, r) >= l) printf("1"); else printf("0");
    }
    return 0;
}
```