---
title: 「Luogu 1412」经营与开发
categories: 题解
tags:
    - 题解
    - 动态规划
    - 洛谷
    - C++
mathjax: true
---

### Portal

Portal1: [Luogu](https://www.luogu.com.cn/problem/P1412)

### Description

你驾驶着一台带有钻头（初始能力值$w$）的飞船，按既定路线依次飞过$n$个星球。


星球笼统的分为$2$类：资源型和维修型。（$p$为钻头当前能力值）

1. 资源型：含矿物质量$\rm a[i]$，若选择开采，则得到$a[i] \times p$的金钱，之后钻头损耗k%，即$p = p \times (1 - 0.01k)$

1. 维修型：维护费用$\rm b[i]$，若选择维修，则支付$b[i] \times p$的金钱，之后钻头修复c%，即$p = p \times (1 + 0.01c)$

注：维修后钻头的能力值可以超过初始值（你可以认为是翻修$+$升级）

金钱可以透支。

请作为舰长的你仔细抉择以最大化收入。

### Input

第一行$4$个整数$n, k, c, w$。

以下$n$行，每行$2$个整数$type, x$。

$\rm type$为$1$则代表其为资源型星球，$x$为其矿物质含量$\rm a[i]$；

$\rm type$为$2$则代表其为维修型星球，$x$为其维护费用$\rm b[i]$；

### Output

一个实数（保留$2$位小数），表示最大的收入。

### Sample Input

```
5 50 50 10
1 10
1 20
2 10
2 20
1 30
```

### Sample Output

```
375.00
```

### Hint

【数据范围】

对于$30\%$的数据$n \le 100$；

另有$20\%$的数据$n \le 1000, k = 100$；

对于$100\%$的数据$n \le 100000, 0 \le k, c, w, a[i], b[i] \le 100$；保证答案不超过$10^9$。

### Solution

很容易想到是动态规划，按题目说的，我们分为两种情况，分别为$\rm type = 1$和$\rm type = 2$，但是如果直接顺序进行$\rm dp$，发现存在后效性，无法直接确定答案，所以我们倒着来，转移方程分别为：

1. 当$type = 1$时：$\rm dp[i] = \max(dp[i + 1], a[i] + dp[i + 1] \times (1 - 0.01 \times k))$

2. 当$type = 2$时，$\rm dp[i] = \max(dp[i + 1], -a[i] + dp[i + 1] \times (1 + 0.01 \times c))$

答案就是$\rm dp[1]$。

### Code

```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>

using namespace std;

const int MAXN = 100005;
int n, k, c, w, typ[MAXN], a[MAXN];
double dp[MAXN];
int main() {
    scanf("%d%d%d%d", &n, &k, &c, &w);
    for (int i = 1; i <= n; i++)
        scanf("%d%d", &typ[i], &a[i]);
    memset(dp, 0, sizeof(dp));
    for (int i = n; i; i--)//注意倒着枚举
        if (typ[i] == 1) dp[i] = max(dp[i + 1], a[i] + dp[i + 1] * (1 - 0.01 * k)); else dp[i] = max(dp[i + 1], -a[i] + dp[i + 1] * (1 + 0.01 * c));//转移
    printf("%.2lf\n", dp[1] * w);//乘以初始的能力值
    return 0;
}
```