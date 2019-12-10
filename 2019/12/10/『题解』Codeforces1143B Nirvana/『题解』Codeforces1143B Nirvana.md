---
title: 『题解』Codeforces1143B Nirvana
categories: 题解
tags:
    - 题解
    - 数论，数学
    - Codeforces
    - 洛谷
    - C++
---

### Portal

Portal1: [Codeforces](https://codeforces.com/problemset/problem/1143/B)

Portal2: [Luogu](https://www.luogu.com.cn/problem/CF1143B)

### Description

Kurt reaches nirvana when he finds the product of all the digits of some positive integer. Greater value of the product makes the nirvana deeper.

Help Kurt find the maximum possible product of digits among all integers from $1$ to $n$.

### Input

The only input line contains the integer $n (1 \le n \le 2 \times 10 ^ 9)$.

### Output

Print the maximum product of digits among all integers from $1$ to $n$.

### Sample Input1

```
390
```

### Sample Output1

```
216
```

### Sample Input2

```
7
```

### Sample Output2

```
7
```

### Sample Input3

```
1000000000
```

### Sample Output3

```
387420489
```

### Solution

我们可以用递归，每次从右边少一位下去，进行递归，如果剩下的数是不为0的一位数，就返回这个数，否则返回1，然后枚举最后一位，选择直接用那一位的数或者退位。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

int n;
inline int f(int x) {
    if (x < 10) {//判断一位数的情况
        if (x == 0) return 1; else return x;
    }
    return max((x % 10) * f(x / 10), 9 * f(x / 10 - 1));//直接用那一位的数或者退位
}
int main() {
    scanf("%d", &n);
    printf("%d\n", f(n));
    return 0;
}
```