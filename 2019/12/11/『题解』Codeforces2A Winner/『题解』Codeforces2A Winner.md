---
title: 『题解』Codeforces2A Winner
categories: 题解
tags:
    - 题解
    - 模拟
    - Codeforces
    - 洛谷
    - C++
mathjax: true
---

### Portal

Portal1: [Codeforces](http://codeforces.com/problemset/problem/2/A)

Portal2: [Luogu](https://www.luogu.com.cn/problem/CF2A)

### Description

The winner of the card game popular in Berland "Berlogging" is determined according to the following rules. If at the end of the game there is only one player with the maximum number of points, he is the winner. The situation becomes more difficult if the number of such players is more than one. During each round a player gains or loses a particular number of points. In the course of the game the number of points is registered in the line "$\texttt{name score}$", where name is a player's name, and score is the number of points gained in this round, which is an integer number. If score is negative, this means that the player has lost in the round. So, if two or more players have the maximum number of points (say, it equals to $m$) at the end of the game, than wins the one *of them* who scored at least $m$ points first. Initially each player has $0$ points. It's guaranteed that at the end of the game at least one player has a positive number of points.

### Input

The first line contains an integer number $n(1 \le n \le 1000)$, $n$ is the number of rounds played. Then follow $n$ lines, containing the information about the rounds in "$\texttt{name score}$" format in chronological order, where $\texttt{name}$ is a string of lower-case Latin letters with the length from $1$ to $32$, and $\texttt{score}$ is an integer number between $-1000$ and $1000$, inclusive.

### Output

Print the name of the winner.

### Sample Input1

```
3
mike 3
andrew 5
mike 2
```

### Sample Output1

```
andrew
```

### Sample Input2

```
3
andrew 3
andrew 2
mike 5
```

### Sample Output2

```
andrew
```

### Description in Chinese

在`Berland`流行着纸牌游戏“`Berlogging`”，这个游戏的赢家是根据以下规则确定的：在每一轮中，玩家获得或失去一定数量的分数，在游戏过程中，分数被记录在“名称和得分”行中，其中名字是玩家的名字，得分是在这一轮中获得的分数。得分是负值意味着玩家失去了相应的分数。如果在比赛结束时只有一名玩家分数最多，他就是获胜者。如果两名或两名以上的玩家在比赛结束时都有最大的分数$m$，那么其中首先获得至少$m$分的玩家胜利。开始时，每个玩家都是$0$分。保证在比赛结束时至少有一个玩家的分数为正。

### Solution

把每个人的得分存到$\texttt{map}$里，然后求出最大得分，再寻找最先到达最大得分的人，即为获胜者。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>
#include<map>

using namespace std;

const int INF = 0x3f3f3f3f, MAXN =  1005;
int n, a[MAXN];
string name[MAXN];
map<string, int> Map1, Map2;
int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        cin >> name[i] >> a[i];
        Map1[name[i]] += a[i];
    }//映射到map里
    int Max = -INF;
    for (int i = 0; i < n; i++)
        if (Map1[name[i]] > Max) Max = Map1[name[i]];//求出最大得分
    for (int i = 0; i < n; i++) {
        Map2[name[i]] += a[i];
        if (Map2[name[i]] >= Max && Map1[name[i]] >= Max) {
            cout << name[i] << endl;//输出最先到最大得分的人
            return 0;
        }
    }
    return 0;
}
```