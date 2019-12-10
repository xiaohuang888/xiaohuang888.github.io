---
title: 『题解』UVa11324 The Largest Clique
categories: 题解
tags:
    - 题解
    - Tarjan
    - 记忆化搜索
    - UVa
    - 洛谷
    - C++
---

### Portal

Portal1：[UVa](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=2299)

Portal2：[Luogu](https://www.luogu.com.cn/problem/UVA11324)

Portal3：[Vjudge](https://vjudge.net/problem/UVA-11324)

### Description

Given a directed graph $\text{G}$, consider the following transformation.
First, create a new graph $\text{T(G)}$ to have the same vertex set as $\text{G}$. Create a directed edge between two vertices u and v in $\text{T(G)}$ if and only if there is a path between u and v in $\text{G}$ that follows the directed edges only in the forward direction. This graph $\text{T(G)}$ is often called the $\text{transitive closure}$ of $\text{G}$.
![](https://i.loli.net/2019/02/23/5c7127b4a548d.png)
We define a $\text{clique}$ in a directed graph as a set of vertices $\text{U}$ such that for any two vertices u and v in $\text{U}$, there is a directed edge either from u to v or from v to u (or both). The size of a clique is the number of vertices in the clique.

### Input

The number of cases is given on the first line of input. Each test case describes a graph $\text{G}$. It begins with a line of two integers $n$ and $m$, where $0 \leq n \leq 1000$ is the number of vertices of $\text{G}$ and $0 \leq m \leq 50, 000$ is the number of directed edges of $\text{G}$. The vertices of $\text{G}$ are numbered from $1$ to $n$. The following $m$ lines contain two distinct integers $u$ and $v$ between $1$ and $n$ which define a directed edge from $u$ to $v$ in $\text{G}$.

### Output

For each test case, output a single integer that is the size of the largest clique in $\text{T(G)}$.

### Sample Input

```
1
5 5
1 2
2 3
3 1
4 1
5 2
```

### Sample Output

```
4
```

### Description in Chinese

给你一张有向图$\text{G}$，求一个结点数最大的结点集，使得该结点集中的任意两个结点 $u$ 和 $v$ 满足：要么 $u$ 可以达 $v$，要么 $v$ 可以达 $u$（$u$, $v$相互可达也行）。

### Solution

`Tarjan`缩点$+$记忆化搜索。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

const int MAXN=200005;
struct node {
    int to, nxt;
} edge[MAXN];
int T, n, m, u, v, num, cnt, top, tot, ans, head[MAXN], DFN[MAXN], LOW[MAXN], sum[MAXN], vis[MAXN], sum1[MAXN], stack[MAXN], belong[MAXN];
inline void addedge(int u, int v) {//前向星存图
    edge[num].to=v; edge[num].nxt=head[u]; head[u]=num; num++;
}
inline void init() {//初始化
    num=cnt=top=tot=ans=0;
    memset(head, -1, sizeof(head));
    memset(DFN, 0, sizeof(DFN));
    memset(LOW, 0, sizeof(LOW));
    memset(vis, 0, sizeof(vis));
    memset(sum, 0, sizeof(sum));
    memset(sum1, -1, sizeof(sum1));
}
inline void tarjan(int u) {//Tarjan缩点
    vis[u]=1;
    stack[++top]=u;
    DFN[u]=++cnt;
    LOW[u]=cnt;
    for (int i=head[u]; ~i; i=edge[i].nxt) {
        int v=edge[i].to;
        if (!DFN[v]) {
            tarjan(v);
            LOW[u]=min(LOW[u], LOW[v]);
        } else
        if (vis[v]) LOW[u]=min(LOW[u], DFN[v]);
    }
    if (DFN[u]==LOW[u]) {
        tot++;
        while (stack[top]!=u) {
            vis[stack[top]]=0;
            belong[stack[top]]=tot;
            sum[tot]++;
            top--;
        }
        vis[stack[top]]=0;
        belong[stack[top]]=tot;
        top--;
        sum[tot]++;
    }
}
inline int dfs(int u) {//记忆化搜索
    if (sum1[u]!=-1) return sum1[u];
    sum1[u]=sum[u];
    int addd=0;
    for (int i=1; i<=n; i++) {
        if (belong[i]==u) {
            for (int j=head[i]; ~j; j=edge[j].nxt) {
                int v=edge[j].to, s1=belong[v];
                if (u==s1) continue;
                addd=max(addd, dfs(s1));
            }
        }
    }
    return sum1[u]+=addd;
}
int main() {
    scanf("%d",&T);
    while (T--) {
        scanf("%d%d",&n, &m);
        init();
        for (int i=1; i<=m; i++) {
            scanf("%d%d",&u, &v);
            addedge(u, v);
        }
        for (int i=1; i<=n; i++)
            if (!DFN[i]) tarjan(i);
        for (int i=1; i<=tot; i++)
            ans=max(ans, dfs(i));//寻找最大值
        printf("%d\n",ans);//输出
    }
    return 0;
}
```