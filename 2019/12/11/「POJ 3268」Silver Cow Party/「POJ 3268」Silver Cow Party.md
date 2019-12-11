---
title: 「POJ 3268」Silver Cow Party
categories: 题解
tags:
    - 题解
    - 最短路
    - dijkstra
    - SPFA
    - POJ
    - C++
mathjax: true
---

### Portal

Portal1: [POJ](http://poj.org/problem?id=3268)

Portal2: [Luogu](https://www.luogu.com.cn/problem/P1821)

<!-- more -->

### Description

One cow from each of N farms $(1  \le  N  \le  1000)$ conveniently numbered $1 \cdots N$ is going to attend the big cow party to be held at farm #X $(1  \le  X  \le  N)$. A total of $M (1  \le  M  \le  100,000)$ unidirectional (one-way roads connects pairs of farms; road $i$ requires $T_i (1  \le  Ti  \le  100)$ units of time to traverse.

Each cow must walk to the party and, when the party is over, return to her farm. Each cow is lazy and thus picks an optimal route with the shortest time. A cow's return route might be different from her original route to the party since roads are one-way.

Of all the cows, what is the longest amount of time a cow must spend walking to the party and back?


寒假到了，$N$头牛都要去参加一场在编号为$X$（$1 \le X \le N$）的牛的农场举行的派对（$1 \le N \le 1000$），农场之间有$M$（$1 \le M \le 100000$）条有向路，每条路长$Ti$（$1 \le Ti \le 100$）。

每头牛参加完派对后都必须回家，无论是去参加派对还是回家，每头牛都会选择最短路径，求这N头牛的最短路径（一个来回）中最长的一条路径长度。

### Input

第一行三个整数$N$，$M$，$X$；

第二行到第$M + 1$行：每行有三个整数$A_i$，$B_i$，$T_i$ ,表示有一条从$A_i$农场到$B_i$农场的道路，长度为$T_i$。

### Output

一个整数，表示最长的最短路得长度。

### Sample Input

```
4 8 2
1 2 4
1 3 2
1 4 7
2 1 1
2 3 5
3 1 2
3 4 4
4 2 3
```

### Sample Output

```
10
```

### Hint

![](https://s2.ax1x.com/2019/09/26/unsDbt.jpg)

### Solution

题目让我们一些奶牛走到一个点，再从那个点走回来的最短路之和的最大值。那么我们直接用`dijkstra`计算两次最短路（走过去，走回来）就可以了，最后判断一下，那头奶牛需要走的路是最长的，然后问题就解决了。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>
#include<queue>

using namespace std;

const int INF = 0x3f3f3f3f, MAXN = 200005;
struct EDGE {
    int nxt, to, val;
} edge[MAXN];
int n, m, S, cnt, U[MAXN], V[MAXN], VAL[MAXN], dis[MAXN], dist[MAXN], head[MAXN];
bool vis[MAXN];
inline void addedge(int u, int v, int val) {//邻接表存图
    edge[++cnt].to = v; edge[cnt].val = val; edge[cnt].nxt = head[u]; head[u] = cnt;
}
inline void dijkstra(int S) {//dijkstra最短路
    memset(dis, INF, sizeof(dis));
    priority_queue< pair<int, int> > Q;
    Q.push(make_pair(0, S));
    dis[S] = 0;
    while (!Q.empty()) {
        int u = Q.top().second;
        Q.pop();
        if (vis[u]) continue;
        vis[u] = 1;
        for (int i = head[u]; ~i; i = edge[i].nxt) {
            int v = edge[i].to;
            if (dis[v] > dis[u] + edge[i].val) {
                dis[v] = dis[u] + edge[i].val;
                Q.push(make_pair(-dis[v], v));
            }
        }
    }
}
int main() {
    scanf("%d%d%d", &n, &m, &S);
    memset(head, -1, sizeof(head));
    for (int i = 1; i <= m; i++) {
        scanf("%d%d%d", &U[i], &V[i], &VAL[i]);
        addedge(U[i], V[i], VAL[i]);//正向建图
    }
    dijkstra(S);
    for (int i = 1; i <= n; i++)
        dist[i] = dis[i];//记录走到目标点的路程
    cnt = 0;
    memset(edge, 0, sizeof(edge));
    memset(vis, 0, sizeof(vis));
    memset(head, -1, sizeof(head));//注意清空数组
    for (int i = 1; i <= m; i++)
        addedge(V[i], U[i], VAL[i]);//反向建图
    dijkstra(S);
    int Max = -INF;
    for (int i = 1; i <= n; i++)
        Max = max(Max, dis[i] + dist[i]);//判断那个奶牛是走得最多的
    printf("%d\n", Max);
    return 0;
}
```