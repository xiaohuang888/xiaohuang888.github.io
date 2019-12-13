---
title: 「Luogu 3792」由乃与大母神原型和偶像崇拜
categories: 题解
tags:
    - 题解
    - 线段树
    - 分块
    - 洛谷
    - C++
mathjax: true
---

### Portal

Portal1: [Luogu](https://www.luogu.com.cn/problem/P3792)

### Description

给你一个序列$a$

每次两个操作：

1. 修改$x$位置的值为$y$；

2. 查询区间$[l, r]$是否可以重排为值域上连续的一段。


### Input

第一行两个数$n, m$；

第二行$n$个数表示$a[i]$；

后面m行每行三个数`opt x y`，或者`opt l r`，代表操作。

### Output

如果可以，输出`damushen`；

否则输出`yuanxing`。

### Sample Input

```
5 5
1 2 3 4 5
2 1 5
2 2 3
2 3 3
1 3 6
2 3 5
```

### Sample Output

```
damushen
damushen
damushen
damushen
```

### Hint

对于$30\%$的数据，$n, m \le 500$；

对于$60\%$的数据，$n, m \le 100000$；

对于$100\%$的数据，$n, m \le 500000$。

值域$10 ^ 9$；

时限：$2s$

### Solution

这题很明显用线段树解决。

题目要求的是更新一个点，查询一个区间是否能够一个等差数列，我们可以线段树维护最小值，最大值以及区间平方和，在查询的时候我们先询问出最小值与最大值，为等差数列的头与尾，那么我们可以算出这个数列的长度，与题目给出的是否一致，不一致就可以输出`yuanxing`。

然后询问线段树的元素的平方和，与计算的头与尾构成的数列的平方和是否一致。

但由于`long long`自然溢出问题，计算时用暴力解决即可。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

typedef long long LL;
const int INF = 0x7f7f7f7f, MAXN = 2000005, MAXM = 500005;
int n, m, l, r, opt, a[MAXM];
namespace Segtree {
    #define lc rt << 1
    #define rc rt << 1 | 1
    struct node {
        int Min, Max;
        LL sum;
    } tree[MAXN];
    inline void pushup(int rt) {
        tree[rt].sum = tree[lc].sum + tree[rc].sum;
        tree[rt].Min = min(tree[lc].Min, tree[rc].Min);
        tree[rt].Max = max(tree[lc].Max, tree[rc].Max);
    }
    inline void pushdown(int rt, int val) {
        tree[rt].sum = val * val;
        tree[rt].Min = val;
        tree[rt].Max = val;
    }
    inline void build(int rt, int l, int r) {//建树
        if (l == r) {
            pushdown(rt, a[l]);
            return ;
        }
        int mid = l + r >> 1;
        build(lc, l, mid);
        build(rc, mid + 1, r);
        pushup(rt);
    }
    inline void update(int rt, int l, int r, int pos, int val) {//线段树更改
        if (l == r) {
            pushdown(rt, val);
            return ;
        }
        int mid = l + r >> 1;
        if (pos <= mid) update(lc, l, mid, pos, val); else update(rc, mid + 1, r, pos, val);
        pushup(rt);
    }
    inline int query_min(int rt, int l, int r, int ansl, int ansr) {//线段树询问区间最小值
        if (ansl <= l && r <= ansr) return tree[rt].Min;
        int mid = l + r >> 1, ret = INF;
        if (ansl <= mid) ret = min(ret, query_min(lc, l, mid, ansl, ansr));
        if (mid < ansr) ret = min(ret, query_min(rc, mid + 1, r, ansl, ansr));
        return ret;
    }
    inline int query_max(int rt, int l, int r, int ansl, int ansr) {//线段树询问区间最大值
        if (ansl <= l && r <= ansr) return tree[rt].Max;
        int mid = l + r >> 1, ret = -INF;
        if (ansl <= mid) ret = max(ret, query_max(lc, l, mid, ansl, ansr));
        if (mid < ansr) ret = max(ret, query_max(rc, mid + 1, r, ansl, ansr));
        return ret;
    }
    inline LL query_sum(int rt, int l, int r, int ansl, int ansr) {//线段树询问区间平方和
        if (ansl <= l && r <= ansr) return tree[rt].sum;
        int mid = l + r >> 1;
        LL ret = 0;
        if (ansl <= mid) ret += query_sum(lc, l, mid, ansl, ansr);
        if (mid < ansr) ret += query_sum(rc, mid + 1, r, ansl, ansr);
        return ret;
    }
}
int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i++)
        scanf("%d", &a[i]);
    Segtree :: build(1, 1, n);
    for (int i = 1; i <= m; i++) {
        scanf("%d", &opt);
        if (opt == 1) {
            scanf("%d%d", &l, &r);
            Segtree :: update(1, 1, n, l, r);
        } else {
            scanf("%d%d", &l, &r);
            int first = Segtree :: query_min(1, 1, n, l, r), last = Segtree :: query_max(1, 1, n, l, r);
            if (r - l != last - first) {
                printf("yuanxing\n");
                continue;
            }
            LL ans = 0;
            for (int i = first; i <= last; i++)
                ans += (LL)(i * i);
            if (ans == Segtree :: query_sum(1, 1, n, l, r)) printf("damushen\n"); else printf("yuanxing\n");
        }
    }
    return 0;
}
```