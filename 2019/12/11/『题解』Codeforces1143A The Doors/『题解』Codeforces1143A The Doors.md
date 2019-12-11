---
title: 『题解』Codeforces1143A The Doors
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

Portal1: [Codeforces](https://codeforces.com/problemset/problem/1143/A)

Portal2: [Luogu](https://www.luogu.com.cn/problem/CF1143A)

<!-- more -->

### Description

Three years have passes and nothing changed. It is still raining in London, and Mr. Black has to close all the doors in his home in order to not be flooded. Once, however, Mr. Black became so nervous that he opened one door, then another, then one more and so on until he opened all the doors in his house.

There are exactly two exits from Mr. Black's house, let's name them left and right exits. There are several doors in each of the exits, so each door in Mr. Black's house is located either in the left or in the right exit. You know where each door is located. Initially all the doors are closed. Mr. Black can exit the house if and only if all doors in at least one of the exits is open. You are given a sequence in which Mr. Black opened the doors, please find the smallest index $k$ such that Mr. Black can exit the house after opening the first $k$ doors.

We have to note that Mr. Black opened each door at most once, and in the end all doors became open.

### Input

The first line contains integer $n (2 \le n \le 200\,000)$ — the number of doors.

The next line contains $n$ integers: the sequence in which Mr. Black opened the doors. The $i$-th of these integers is equal to $0$ in case the $i$-th opened door is located in the left exit, and it is equal to $1$ in case it is in the right exit.

It is guaranteed that there is at least one door located in the left exit and there is at least one door located in the right exit.

### Output

Print the smallest integer $k$ such that after Mr. Black opened the first $k$ doors, he was able to exit the house.

### Sample Input1

```
5
0 0 1 0 0
```

### Sample Output1

```
3
```

### Sample Input2

```
4
1 0 0 1
```

### Sample Output2

```
3
```

### Hint

In the first example the first two doors are from the left exit, so when Mr. Black opened both of them only, there were two more closed door in the left exit and one closed door in the right exit. So Mr. Black wasn't able to exit at that moment.

When he opened the third door, all doors from the right exit became open, so Mr. Black was able to exit the house.

In the second example when the first two doors were opened, there was open closed door in each of the exit.

With three doors opened Mr. Black was able to use the left exit.

### Solution

题目很简单，实际上就是让我们从$n - 1$倒着枚举，求最后一个与数列最后的一个数字不同的是哪一个，输出它的编号就可以了。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

const int MAXN = 200005;
int n, x, a[MAXN];
int main() {
    scanf("%d", &n);
    for (int i = 1; i <= n; i++)
        scanf("%d", &a[i]);
    int x = a[n];//记录最后一个数字
    for (int i = n - 1; i >= 1; i--)
        if (a[i] != x) {//找出与数列最后一个数字的数
            printf("%d\n", i);
            return 0;
        }
    return 0;
}
```