---
title: 『题解』Coderforces352A Jeff and Digits
categories: 题解
tags:
    - 题解
    - 数论，数学
    - Codeforces
    - 洛谷
    - C++
mathjax: true
---

### Portal

Portal1: [Codeforces](http://codeforces.com/problemset/problem/352/A)

Portal2: [Luogu](https://www.luogu.com.cn/problem/CF352A)

<!-- more -->

### Description

Jeff's got n cards, each card contains either digit 0, or digit 5. Jeff can choose several cards and put them in a line so that he gets some number. What is the largest possible number divisible by 90 Jeff can make from the cards he's got?

Jeff must make the number without leading zero. At that, we assume that number 0 doesn't contain any leading zeroes. Jeff doesn't have to use all the cards.
### Input

The first line contains integer $n (1 \le n \le 103)$. The next line contains $n$ integers $a_1, a_2, \cdots , a_n (a_i = 0 or a_i = 5)$. Number ai represents the digit that is written on the $i$-th card.

### Output

In a single line print the answer to the problem - the maximum number, divisible by 90. If you can't make any divisible by 90 number from the cards, print -1.

### Sample Input1

```

4
5 0 5 0
```

### Sample Output1

```
0
```

### Sample Input2

```
11
5 5 5 5 5 5 5 5 0 5 5
```

### Sample Output2

```
5555555550
```

### Sample Explain

In the first test you can make only one number that is a multiple of 90 - 0.

In the second test you can make number $5555555550$, it is a multiple of 90.

### Description in Chinese

`Jeff`有一些数字卡片，上面为$0$或$5$。`Jeff`想用这些卡片组成一个数字，使这个数字可以被$90$整除并且尽可能的大。

### Solution

先分解质因数：$90 = 9 \times 10$

为什么不分成$90 = 2 \times 3^2 \times 5$呢，因为$9$有自己的整除特性：各个数位之和被$9$整除。

$10$的整除特性就不用说了。

然后分类讨论：

1. 当$0$的个数为$0$，也就是没有$0$，我们就直接输出$-1$，因为无论怎么取都不能被$10$整除。

1. 当$5$的个数$\le 9$，而且存在$0$时，我们只能构造出$9 | 0$（这里的$|$表示整除），比如样例$1$。

1. 当$5$的个数$\ge 9$，而且存在$0$时，就可以构造能被$90$的数，比如样例$2$。

我们可以把$5$的个数$\div 9$取整后$\times 9$就得到所要输出的$5$的个数了，这个数一定是最大的能被9整除的数，当然最后要把所有的$0$输出。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

int n, x, sum1, sum2;
int main() {
    scanf("%d", &n);
    for (int i = 1; i <= n; i++) {
        scanf("%d", &x);
        if (x == 5) sum1++; else sum2++;//统计0和5的数量
    }
    if (sum2 == 0) {//情况1
        printf("-1\n");
        return 0;
    }
    if (sum1 < 9) {//情况2
        printf("0\n");
        return 0;
    }
    sum1 = sum1 / 9 * 9;//将5的个数÷9整除后×9，得到最终的5的个数
    for (int i = 1; i <= sum1; i++)
        printf("5");
    for (int i = 1; i <= sum2; i++)
        printf("0");
    printf("\n");
    return 0;
}
```