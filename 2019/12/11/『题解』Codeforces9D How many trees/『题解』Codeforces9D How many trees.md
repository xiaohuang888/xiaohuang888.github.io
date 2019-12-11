---
title: 『题解』Codeforces9D How many trees?
categories: 题解
tags:
    - 题解
    - 动态规划
    - Codeforces
    - 洛谷
    - C++
mathjax: true
---

### Portal

Portal1: [Codeforces](http://codeforces.com/problemset/problem/9/D)

Portal2: [Luogu](https://www.luogu.com.cn/problem/CF9D)

<!-- more -->

### Description

In one very old text file there was written Great Wisdom. This Wisdom was so Great that nobody could decipher it, even Phong - the oldest among the inhabitants of Mainframe. But still he managed to get some information from there. For example, he managed to learn that User launches games for pleasure - and then terrible Game Cubes fall down on the city, bringing death to those modules, who cannot win the game...

For sure, as guard Bob appeared in Mainframe many modules stopped fearing Game Cubes. Because Bob (as he is alive yet) has never been defeated by User, and he always meddles with Game Cubes, because he is programmed to this.

However, unpleasant situations can happen, when a Game Cube falls down on Lost Angles. Because there lives a nasty virus - Hexadecimal, who is... mmm... very strange. And she likes to play very much. So, willy-nilly, Bob has to play with her first, and then with User.

This time Hexadecimal invented the following entertainment: Bob has to leap over binary search trees with n nodes. We should remind you that a binary search tree is a binary tree, each node has a distinct key, for each node the following is true: the left sub-tree of a node contains only nodes with keys less than the node's key, the right sub-tree of a node contains only nodes with keys greater than the node's key. All the keys are different positive integer numbers from $1$ to $n$. Each node of such a tree can have up to two children, or have no children at all (in the case when a node is a leaf).

In Hexadecimal's game all the trees are different, but the height of each is not lower than h. In this problem «height» stands for the maximum amount of nodes on the way from the root to the remotest leaf, the root node and the leaf itself included. When Bob leaps over a tree, it disappears. Bob gets the access to a Cube, when there are no trees left. He knows how many trees he will have to leap over in the worst case. And you?

### Input

The input data contains two space-separated positive integer numbers $n$ and $h (n \le 35, h \le n)$.

### Output

Output one number - the answer to the problem. It is guaranteed that it does not exceed $9 \times 10^18$.

### Sample Input1

```
3 2
```

### Sample Output1

```
5
```

### Sample Input2

```
3 3
```

### Sample Output2

```
4
```

### Description in Chinese

用$n$个点组成二叉树，问高度大于等于$h$的有多少个。

### Solution

这题$n$的范围很小，只有$35$，所以我们用$O(n^4)$的动态规划来解决。

`dp[i][j]`表示用$i$个点，构成高度为$j$的二叉树的个数。

动态转移方程为$dp[i][max(j, k) + 1] += dp[L][j] \times dp[R][k]$，其中$i$表示所用的结点数，$j$是枚举左子树的结点数，$L$表示左子树的结点总数，$k$是枚举左子树的结点数，$R$表示右子树的结点总数。

答案就是$\sum^{n}_{i = h}{dp[n][i]}$。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

typedef long long LL;
const int MAXN = 40;
int n, h;
LL dp[MAXN][MAXN];
int main() {
    scanf("%d%d", &n, &h);
    dp[0][0] = 1;//用0个结点，可以构造出1棵深度为0的二叉树
    for (int i = 1; i <= n; i++)//枚举总结点数
        for (int L = 0; L < i; L++) {//枚举左子树的结点数
            int R = i - L - 1;//计算剩下的结点数，再减去根结点，就是右子树的结点数
            for (int j = 0; j <= L; j++)//左子树
                for (int k = 0; k <= R; k++)//右子树
                    dp[i][max(j, k) + 1] += dp[L][j] * dp[R][k];//因为是累计可行方案所以是乘法
        }
    LL ans = 0;
    for (int i = h; i <= n; i++)
        ans += dp[n][i];//累计答案
    printf("%lld\n", ans);
    return 0;
}
```