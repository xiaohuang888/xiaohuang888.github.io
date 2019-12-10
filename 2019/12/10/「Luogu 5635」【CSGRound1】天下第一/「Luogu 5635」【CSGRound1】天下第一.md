---
title: 「Luogu 5635」【CSGRound1】天下第一
categories: 题解
tags:
    - 题解
    - 记忆化搜索
    - 洛谷
    - C++
---

### Portal

Portal1: [Luogu](https://www.luogu.com.cn/problem/P5635)

### Description

给定两个数$x$,$y$，与一个模数$Mod$。

`cbw`拥有数$x$，`zwc`拥有数$y$。

第一个回合：$x = (x + y) \% Mod$

第二个回合：$y = (x + y) \% Mod$

第三个回合：$x = (x + y) \% Mod$

第四个回合：$y = (x + y) \% Mod$

以此类推$\cdots \cdots$

如果$x$先到$0$，则`cbw`胜利。如果$y$先到$0$，则`zwc`胜利。如果$x, y$都不能到$0$，则为平局。

`cbw`为了捍卫自己主席的尊严，想要提前知道游戏的结果，并且可以趁机动点手脚，所以他希望你来告诉他结果。

### Input

有多组数据：

第一行：$T$和$Mod$ 表示一共有$T$组数据且模数都为$Mod$；

以下$T$行，每行两个数$x, y$。


### Output

$1$表示`cbw`获胜，$2$表示`zwc`获胜，$error$表示平局。

### Sample Input1

```
1 10
1 3
```

### Sample Output1

```
error
```

### Sample Input2

```
1 10
4 5
```

### Sample Output2

```
1
```

### Hint

对于$5 \%$的数据，满足输出全是$error$；

对于$100 \%$的数据，满足$1 \le T \le 200$，$1 \le X, Y, MOD \le 10000$。

### Solution

这题很容易看出来是记忆化搜索，我们对于每一次询问的每一次答案都记录下来，在下一次的搜索中，如果有这个答案了，就直接输出，否则继续搜索并记录，直到出现过为止。

但是$10000 \times 10000$的$\rm{int}$数组会$\rm{MLE}$，于是我们可以用$\rm{short}$类型来存储记忆化搜索的结果。

**附：$10000 \times 10000$的$\rm{int}$类型的数组内存是$\rm{381.47MB}$，$10000 \times 10000$的$\rm{short}$类型的数组内存是$\rm{190.735MB}$。所以此题用$\rm{int}$类型会爆空间**

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

const int MAXN = 10005;
int T, mod, x, y;
short dp[MAXN][MAXN];
inline short solve(int x, int y, int cnt) {//记忆化搜索
    if (cnt >= 10000) return 0;//cnt表示循环次数，以判断平局的情况
    if (dp[x][y]) return dp[x][y];
    if (x == 0) return 1;
    if (y == 0) return 2;
    return dp[x][y] = solve((x + y) % mod, ((x + y) % mod + y) % mod, cnt + 1);
}
int main() {
    scanf("%d%d", &T, &mod);
    while (T--) {
        scanf("%d%d", &x, &y);
        int ans = solve(x, y, 0);
        if (ans == 1) printf("1\n"); else
        if (ans == 2) printf("2\n"); else printf("error\n");
    }
    return 0;
}
```