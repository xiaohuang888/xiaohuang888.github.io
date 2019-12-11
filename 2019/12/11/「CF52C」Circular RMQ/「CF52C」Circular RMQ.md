---
title: 「CF52C」Circular RMQ
categories: 题解
tags:
    - 题解
    - 线段树
    - 分块
    - 洛谷
    - Codeforces
    - C++
mathjax: true
---

### Portal

Portal1: [Codeforces](http://codeforces.com/problemset/problem/52/C)

Portal2: [Luogu](http://codeforces.com/problemset/problem/52/C)

<!-- more -->

### Description

You are given circular array $a_0, a_1, \cdots, a_{n - 1}$. There are two types of operations with it:

+ $\textrm{inc}(lf, rg, v)$ — this operation increases each element on the segment $[lf, rg]$ (inclusively) by $v$;

+ $\textrm{rmq}(lf, rg)$ — this operation returns minimal value on the segment $[lf, rg]$ (inclusively).

Assume segments to be circular, so if $n = 5$ and $lf = 3, rg = 1$, it means the index sequence: $3, 4, 0, 1$.

Write program to process given sequence of operations.

### Input

The first line contains integer $n (1 \le n \le 200000)$. The next line contains initial state of the array: $a_0, a_1, \cdots, a_{n - 1} ( -10^6 \le ai \le 10^6)$, $a_i$ are integer. The third line contains integer $m (0 \le m \le 200000)$, $m$ — the number of operartons. Next $m$ lines contain one operation each. If line contains two integer $lf, rg (0 \le lf, rg \le n - 1)$ it means rmq operation, it contains three integers $lf, rg, v (0 \le lf, rg \le n - 1; -10^6 \le v \le 10^6)$ — inc operation.

### Output

For each rmq operation write result for it. Please, do not use `%lld` specificator to read or write $64$-bit integers in C++. It is preffered to use cout (also you may use `%I64d`).

### Sample Input

```
4
1 2 3 4
4
3 0
3 0 -1
0 1
2 1
```

### Sample Output

```
1
0
0
```

### Solution

我们可以用线段树来解决区间`RMQ`问题，我们在线段树上维护一个最小值与懒标记，这样问题就解决了。

读入的时候我们可以判断后面一个字符是不是空格，可以直接在快速读入里判断，这样就可以判断出一行有三个数还是两个数。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

const int MAXN = 200005;
int n, m, l, r, val, a[MAXN];
bool opt;
namespace Segtree {
    #define ls rt << 1
    #define rs rt << 1 | 1
    typedef long long LL;
    const LL Seg_INF = 1e18;
    const int Seg_MAXN = 1000005;
    struct SMT {
        LL Min, tag;
    } tree[Seg_MAXN];
    inline void build(int rt, int l, int r) {//建立线段树
        if (l == r) {
            tree[rt].Min = a[l];
            return ;
        }
        int mid = l + r >> 1;
        build(ls, l, mid);
        build(rs, mid + 1, r);
        tree[rt].Min = min(tree[ls].Min, tree[rs].Min);
    }
    inline void update(int rt, int l, int r, int ansl, int ansr, int val) {//线段树修改
        if (ansl <= l && r <= ansr) {
            tree[rt].tag += val;
            return ;
        }
        int mid = l + r >> 1;
        if (ansl <= mid) update(ls, l, mid, ansl, ansr, val);
        if (mid < ansr) update(rs, mid + 1, r, ansl, ansr, val);
        tree[rt].Min = min(tree[ls].Min + tree[ls].tag, tree[rs].Min + tree[rs].tag);
    }
    inline LL query(int rt, int l, int r, int ansl, int ansr) {//线段树查询
        if (ansl <= l && r <= ansr) return tree[rt].Min + tree[rt].tag;
        int mid = l + r >> 1;
        LL ret = Seg_INF;
        if (ansl <= mid) ret = min(ret, query(ls, l, mid, ansl, ansr));
        if (mid < ansr) ret = min(ret, query(rs, mid + 1, r, ansl, ansr));
        return ret + tree[rt].tag;
    }
}

using namespace Segtree;

inline int read() {
    opt = 0;
    char ch = getchar();
    int x = 0, f = 1;
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while ('0' <= ch && ch <= '9') {
        x = (x << 1) + (x << 3) + ch - '0';
        ch = getchar();
    }
    if (ch == ' ') opt = 1;//判断空格
    return x * f;
}
int main() {
    n = read();
    for (int i = 1; i <= n; i++)
        a[i] = read();
    build(1, 1, n);
    m = read();
    for (int i = 1; i <= m; i++) {
        l = read(); r = read(); l++; r++;
        if (!opt) {
            if (l <= r) printf("%lld\n", query(1, 1, n, l, r)); else printf("%lld\n", min(query(1, 1, n, l, n), query(1, 1, n, 1, r)));
        } else {
            val = read();
            if (l <= r) update(1, 1, n, l, r, val); else {
                update(1, 1, n, l, n, val);
                update(1, 1, n, 1, r, val);
            }
        }
    }
    return 0;
}
```