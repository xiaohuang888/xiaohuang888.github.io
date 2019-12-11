---
title: 『题解』Coderforces142B Help General
categories: 题解
tags:
    - 题解
    - 贪心
    - 构造
    - Codeforces
    - 洛谷
    - C++
mathjax: true
---

### Portal

Portal1: [Codeforces](http://codeforces.com/problemset/problem/142/B)

Portal2: [Luogu](https://www.luogu.com.cn/problem/CF142B)

<!-- more -->

### Description

Once upon a time in the Kingdom of Far Far Away lived Sir Lancelot, the chief Royal General. He was very proud of his men and he liked to invite the King to come and watch drill exercises which demonstrated the fighting techniques and tactics of the squad he was in charge of. But time went by and one day Sir Lancelot had a major argument with the Fairy Godmother (there were rumors that the argument occurred after the general spoke badly of the Godmother's flying techniques. That seemed to hurt the Fairy Godmother very deeply).

As the result of the argument, the Godmother put a rather strange curse upon the general. It sounded all complicated and quite harmless: <u>"If the squared distance between some two soldiers equals to $5$, then those soldiers will conflict with each other!"</u>

The drill exercises are held on a rectangular n × m field, split into nm square $1 \times 1$ segments for each soldier. Thus, the square of the distance between the soldiers that stand on squares $(x1, y1)$ and $(x2, y2)$ equals exactly $(x1 - x2)^2 + (y1 - y2)^2$. Now not all nm squad soldiers can participate in the drill exercises as it was before the Fairy Godmother's curse. Unless, of course, the general wants the soldiers to fight with each other or even worse... For example, if he puts a soldier in the square $(2, 2)$, then he cannot put soldiers in the squares $(1, 4)$, $(3, 4)$, $(4, 1)$ and $(4, 3)$ — each of them will conflict with the soldier in the square $(2, 2)$.

Your task is to help the general. You are given the size of the drill exercise field. You are asked to calculate the maximum number of soldiers that can be simultaneously positioned on this field, so that no two soldiers fall under the Fairy Godmother's curse.

### Input

The single line contains space-separated integers $n$ and $m (1 \le n, m \le 1000)$ that represent the size of the drill exercise field.

### Output

Print the desired maximum number of warriors.

### Sample Input1

```
2 4
```

### Sample Output1

```
4
```
### Sample Input2

```
3 4
```

### Sample Output2

```
6
```

### Sample Explain

In the first sample test Sir Lancelot can place his $4$ soldiers on the $2 \times 4$ court as follows (the soldiers' locations are marked with gray circles on the scheme):

![](https://s3.ax2x.com/2019/08/01/f8d21b382f1ee770da67f4937338987796ee8283.png)

In the second sample test he can place $6$ soldiers on the $3 \times 4$ site in the following manner:

![](https://s3.ax2x.com/2019/08/01/c737d25d82c2d42966a5c1ef6d8bf6fd7f7177ac.png)

### Description in Chinese

在$n \times m$的国际象棋棋盘上放尽可能多的马，让它们互不攻击，求出最多可以放的马的数量。

### Solution

不妨令$n < m$。

然后，我们分情况来考虑：

1. 当$n = 1$时，无论马放在哪里，都不能攻击到其它任何马，所以答案就是$m$；

1. 当$n = 2$时，我们可以这么放：

```
11001100...
11001100...
```

（`1`表示放，`0`表示不放）

这样能保证马互不攻击，因为这是国际象棋，不存在马因特殊情况不能攻击另一匹马。

![](https://s2.ax1x.com/2019/08/01/eUG336.png)

箭头表示攻击到的点。

3. 当$n > 2$时，我们可以按国际象棋的棋盘放，这样马也不能互相攻击。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

int n, m;
int main() {
    scanf("%d%d", &n, &m);
    if (n > m) swap(n, m);//强制使n < m
    int ans;
    if (n == 1) ans = m; else//情况1
    if (n == 2) {//情况2
        if (m % 4 <= 2) ans = m + m % 4; else ans = m + 1;
    } else ans = (n * m + 1) / 2;//情况3
    printf("%d\n", ans);
    return 0;
}
```