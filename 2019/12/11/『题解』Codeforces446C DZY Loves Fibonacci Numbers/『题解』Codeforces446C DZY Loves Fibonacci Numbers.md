---
title: 『题解』Codeforces446C DZY Loves Fibonacci Numbers
categories: 题解
tags:
    - 题解
    - 线段树
    - Codeforces
    - 洛谷
    - C++
mathjax: true
---

### Portal

Portal1: [Codeforces](http://codeforces.com/problemset/problem/446/C)

Portal2: [Luogu](https://www.luogu.com.cn/problem/CF446C)

<!-- more -->

### Description

In mathematical terms, the sequence $F_n$ of Fibonacci numbers is defined by the recurrence relation

$$F_1 = 1; F_2 = 1; F_n = F_n - 1 + F_n - 2 (n > 2)$$

`DZY` loves Fibonacci numbers very much. Today `DZY` gives you an array consisting of $n$ integers: $a1, a2, \cdots , an$. Moreover, there are $m$ queries, each query has one of the two types:

1. Format of the query "`1 l r`". In reply to the query, you need to add $F_i - l + 1$ to each element ai, where $l \le i \le r$.

1. Format of the query "`2 l r`". In reply to the query you should output the value of  modulo $1000000009 (10^9 + 9)$.

Help `DZY` reply to all the queries.

### Input

The first line of the input contains two integers $n$ and $m (1 \le n, m \le 300000)$. The second line contains $n$ integers $a_1, a_2, \cdots , a_n (1 \le ai \le 10^9)$ — initial array $a$ .

Then, $m$ lines follow. A single line describes a single query in the format given in the statement. It is guaranteed that for each query inequality $1 \le l \le r \le n$ holds.

### Output

For each query of the second type, print the value of the sum on a single line.

### Sample Input

```
4 4
1 2 3 4
1 1 4
2 1 4
1 2 4
2 1 3
```

### Sample Output

```
17
12
```

### Sample Explain

After the first query, $a = [2, 3, 5, 7]$.

For the second query, $sum = 2 + 3 + 5 + 7 = 17$.

After the third query, $a = [2, 4, 6, 9]$.

For the fourth query, $sum = 2 + 4 + 6 = 12$.

### Description in Chinese

题目让我们求给你一个序列，支持区间加`Fibonacci`数列前`r - l + 1`项和查询区间和。

### Solution

一些约定：把斐波那契数列的前两个数$F_1 = 1, F_2 = 1$换成另两个数，仍满足$F_n = F_{n - 1} + F_{n - 2}(n > 2)$的数列称为广义斐波那契数列。

`Fibonacci`数列有一些性质：

性质$1$. $F_n = (\sum^{n - 2}_{i = 1}{F_i}) + F_2(n > 2)$；

证明如下：

首先将前几项`Fibonacci`数列展开。

```
F(1) = 1
F(2) = 1
F(3) = F(1) + F(2)
F(4) = F(2) + F(3) = F(2) + F(1) + F(2)
F(5) = F(3) + F(4) = F(3) + F(2) + F(1) + F(2)
F(6) = F(4) + F(5) = F(4) + F(3) + F(2) + F(1) + F(2)
......
```

在$F_n = F_{n - 1} + F_{n - 2}$中，我们可以把$F_{n - 1}$按式子展开，可得$F_n = \sum^{n - 3}_{i = 1}{F_i} + F_2 + F_{n - 2}$，即$F_n = (\sum^{n - 2}_{i = 1}{F_i}) + F_2(n > 2)$，跟原式一模一样，故原式正确性得证。

性质$2$. 一个广义斐波那契数列数列$f_i$， 当$f_1 = x, f_2 = y$时，则有$f_n = x \times f_{n - 1} + y \times f_{n - 2}$

证明如下：

这个性质与性质`1`类似，证明方法也与性质`1`类似，列举几个：

```
f(1) = x
f(2) = y
f(3) = f(1) + f(2) = x × F(1)
f(4) = f(2) + f(3) = x × F(1) + y × F(2)
f(5) = f(3) + f(4) = x × F(2) + y × F(3)
f(6) = f(4) + f(5) = x × F(3) + y × F(4)
......
```

把上述规律推广到代数式：

$$\begin{align} f_n & = f_{n - 1} + f_{n - 2} \\\ & = x \times f_{n - 2} + y \times f_{n - 3} + x \times f_{n - 3} + y \times f_{n - 4} \\\ & = x \times (f_{n - 2} + f_{n - 3}) + y \times (f_{n - 3} + f_{n - 4}) \\\ & = x \times f_{n - 1} + y \times f_{n - 2} \end{align}$$

证毕。

性质$3$. 任意两段不同的广义斐波那契数列段相加（逐项相加），所得的数列任然是广义斐波那契数列。

这个性质易证。

---

这题我们维护一棵线段树，线段树需要维护$L$至$R$区间的广义斐波那契数列的第一项，第二项与区间的和。

下传标记时，我们可以在左区间加广义斐波那契数列的前两项，在右区间可以求出总和再加上总和就行了，时间复杂$\text{O(n log n)}$。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

typedef long long LL;
const int MAXN = 300005, MAXM = 1200005, mod = 1e9 + 9;
struct node {
    int c1, c2, sum;
} tree[MAXM];
int n, m, opt, x, y, a[MAXN], f[MAXN];
inline int add(int x, int y) {//两项相加并取模
    int ret = x + y;
    if (ret < 0) return ret += mod; else return ret % mod;
}
inline int calc1(int x, int y, int len) {//计算斐波那契
    if (len == 1) return x; else
    if (len == 2) return y; else return ((LL)x * f[len - 2] + (LL)y * f[len - 1]) % mod;
}
inline int calc2(int x, int y, int len) {//计算总和
    if (len == 1) return x; else
    if (len == 2) return add(x, y); else return add(calc1(x, y, len + 2), -y);
}
inline void pushup(int rt) {
    tree[rt].sum = add(tree[rt << 1].sum, tree[rt << 1 | 1].sum);
}
inline void pushdown(int rt, int l, int r) {//下传标记
    if (tree[rt].c1) {
        int mid = l + r >> 1;
        tree[rt << 1].c1 = add(tree[rt << 1].c1, tree[rt].c1);
        tree[rt << 1].c2 = add(tree[rt << 1].c2, tree[rt].c2);
        tree[rt << 1].sum = add(tree[rt << 1].sum, calc2(tree[rt].c1, tree[rt].c2, mid - l + 1));
        int x = calc1(tree[rt].c1, tree[rt].c2, mid - l + 2), y = calc1(tree[rt].c1, tree[rt].c2, mid - l + 3);
        tree[rt << 1 | 1].c1 = add(tree[rt << 1 | 1].c1, x);
        tree[rt << 1 | 1].c2 = add(tree[rt << 1 | 1].c2, y);
        tree[rt << 1 | 1].sum = add(tree[rt << 1 | 1].sum, calc2(x, y, r - mid));
        tree[rt].c1 = 0; tree[rt].c2 = 0;
    }
}
inline void update(int rt, int l, int r, int ansl, int ansr) {//线段树区间更新
    if (ansl <= l && r <= ansr) {
        tree[rt].c1 = add(tree[rt].c1, f[l - ansl + 1]);
        tree[rt].c2 = add(tree[rt].c2, f[l - ansl + 2]);
        tree[rt].sum = add(tree[rt].sum, calc2(f[l - ansl + 1], f[l - ansl + 2], r - l + 1));
        return ;
    }
    pushdown(rt, l, r);
    int mid = l + r >> 1;
    if (ansl <= mid) update(rt << 1, l, mid, ansl, ansr);
    if (ansr > mid) update(rt << 1 | 1, mid + 1, r, ansl, ansr);
    pushup(rt);
}
inline int query(int rt, int l, int r, int ansl, int ansr) {//线段树区间查询
    int ret = 0;
    if (ansl <= l && r <= ansr) {
        ret = tree[rt].sum;
        return ret;
    }
    pushdown(rt, l, r);
    int mid = l + r >> 1;
    if (ansl <= mid) ret = add(ret, query(rt << 1, l, mid, ansl, ansr));
    if (ansr > mid) ret = add(ret, query(rt << 1 | 1, mid + 1, r, ansl, ansr));
    return ret;
}
int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i++) {
        scanf("%d", &x);
        a[i] = add(a[i - 1], x);
    }
    f[1] = 1; f[2] = 1;
    for (int i = 3; i <= n + 2; i++)
        f[i] = add(f[i - 1], f[i - 2]);
    for (int i = 1; i <= m; i++) {
        scanf("%d%d%d", &opt, &x, &y);
        if (opt == 1) update(1, 1, n, x, y); else printf("%d\n", add(query(1, 1, n, x, y), a[y] - a[x - 1]));
    }
    return 0;
}
```