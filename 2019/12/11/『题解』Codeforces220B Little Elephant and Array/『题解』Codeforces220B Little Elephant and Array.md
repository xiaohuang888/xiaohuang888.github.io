---
title: 『题解』Codeforces220B Little Elephant and Array
categories: 题解
tags:
    - 题解
    - 莫队
    - Codeforces
    - 洛谷
    - C++
mathjax: true
---

### Portal

Portal1: [Codeforces](http://codeforces.com/problemset/problem/220/B)

Portal2: [Luogu](https://www.luogu.com.cn/problem/CF220B)

### Description

The Little Elephant loves playing with arrays. He has array $a$, consisting of $n$ positive integers, indexed from $1$ to $n$. Let's denote the number with index $i$ as $a_i$.

Additionally the Little Elephant has $m$ queries to the array, each query is characterised by a pair of integers $l_j$ and $r_j (1 \le l_j \le r_j \le n)$. For each query $l_j, r_j$ the Little Elephant has to count, how many numbers $x$ exist, such that number $x$ occurs exactly $x$ times among numbers $a_{l_j}, a_{l_j + 1}, \cdots , a_{r_j}$.

Help the Little Elephant to count the answers to all queries.

### Input

The first line contains two space-separated integers $n$ and $m (1 \le n, m \le 105)$ - the size of array a and the number of queries to it. The next line contains $n$ space-separated positive integers $a_1, a_2, \cdots , a_n (1 \le ai \le 10^9)$. Next $m$ lines contain descriptions of queries, one per line. The $j$-th of these lines contains the description of the $j$-th query as two space-separated integers $l_j$ and $r_j (1 \le l_j \le r_j \le n)$.

### Output

In $m$ lines print $m$ integers - the answers to the queries. The $j$-th line should contain the answer to the $j$-th query.

### Sample Input1

```
7 2
3 1 2 2 3 3 7
1 7
3 4
```

### Sample Output

```
3
1
```

### Description in Chinese

小象喜欢和数组玩。现在有一个数组$a$，含有$n$个正整数，记第$i$个数为$a_i$。

现在有$m$个询问，每个询问包含两个正整数$l_j$和$r_j (1 \le l_j \le r_j \le n)$，小象想知道在$a_{l_j}到$a_{r_j}$之中有多少个数$x$，其出现次数也为$x$。

### Solution

我们先看题目，发现只有查询，没有修改，所以可以用普通的莫队解决。

题目中的$a_i$的范围是$\in [1, 10^9]$，而数的总数的范围是$\in [1, 10^5]$，所以当这个数大于$10^5$了，这个数就不可能为所求的$x$忽略这个数后就不用进行离散化了。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

const int MAXN = 100005;
int n, m, nowans, a[MAXN], bl[MAXN], ans[MAXN], cnt[MAXN];
struct node {
    int l, r, id;
    bool operator < (const node &x) const {//排序的规则
        return bl[l] == bl[x.l] ? r < x.r : bl[l] < bl[x.l];
    }
} info[MAXN];
inline void add(int x) {
    if (a[x] > n) return ;//如果数值超了n的范围就退出
    if (cnt[a[x]] == a[x]) nowans--;
    cnt[a[x]]++;
    if (cnt[a[x]] == a[x]) nowans++;
}
inline void dec(int x) {
    if (a[x] > n) return ;
    if (cnt[a[x]] == a[x]) nowans--;
    cnt[a[x]]--;
    if (cnt[a[x]] == a[x]) nowans++;
}
int main() {
    scanf("%d%d", &n, &m);
    int block = (int)sqrt(n);
    for (int i = 1; i <= n; i++) {
        scanf("%d", &a[i]);
        bl[i] = i / block;
    }
    for (int i = 1; i <= m; i++) {
        scanf("%d%d", &info[i].l, &info[i].r);//莫队是离线算法，需要记录询问的左右端点
        info[i].id = i;//记录每个询问的编号
    }
    sort(info + 1, info + m + 1);
    memset(cnt, 0, sizeof(cnt));
    int    l = 1, r = 0;
    for (int i = 1; i <= m; i++) {//莫队
        while (l < info[i].l) dec(l++);
        while (l > info[i].l) add(--l);
        while (r < info[i].r) add(++r);
        while (r > info[i].r) dec(r--);
        ans[info[i].id] = nowans;
    }
    for (int i = 1; i <= m; i++)
        printf("%d\n", ans[i]);
    return 0;
}
```