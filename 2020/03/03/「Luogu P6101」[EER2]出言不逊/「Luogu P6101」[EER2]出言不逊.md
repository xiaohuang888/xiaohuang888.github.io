---
title: 「Luogu P6101」[EER2]出言不逊
categories: 题解
tags:
    - 题解
    - 数学，数论
    - 洛谷
    - C++
mathjax: true
---

### Portal

Portal1: [Luogu](https://www.luogu.com.cn/problem/P6101)

### Solution

模拟，先找到在读入字符串内出现次数最多的字符，记录个数，然后以 $2$ 为指数在现有长度上递增，就可以算出答案。

但是`long long`会溢出，所以要判断一下，如`mx + mx < mx`说明已经溢出了，然后就退出答案做个标记，输出的时候$+1$，否则会死循环，至于`__int128`，我没试过。

代码纯属是为过而过，没什么可看的。

### Code

```cpp
#include<bits/stdc++.h>
#pragma GCC optimize(2)
//不知道为什么，不开会T
using namespace std;

char s[1000005];
unsigned long long L;
int main() {
    cin >> s;
    cin >> L;
    sort(s, s + strlen(s));//字符串排序方便计算出现最多的数
    unsigned long long lst = 0, mx = 0;
    s[strlen(s)] = '!';//最后要填一个随意的（不会出现的）字符，不然最后一段就不会算进去
    for (int i = 1; i <= strlen(s); i++)
        if (s[i] != s[i - 1]) {
            if (i - lst > mx) mx = i - lst;
            lst = i;
        }
    unsigned long long now = strlen(s) - 1, ans = 0;
    bool flag = 0;
    while (now < L) {
        now += mx;
        if (mx + mx < mx) {flag = 1; break;}
        mx = mx * 2;
        ans++;
    }
    if (flag) printf("%lld\n", ans + 1); else printf("%lld\n", ans);//溢出要标记
    return 0;
}
```