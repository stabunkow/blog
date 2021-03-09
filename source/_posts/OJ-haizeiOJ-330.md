---
title: OJ-haizeiOJ-330
date: 2021-03-09 18:27:41
mathjax: true
tag: [OJ, haizeiOJ, 树状数组]
---

# [加强的整数问题](http://oj.haizeix.com/problem/330)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210309162947998.png)

**输入**

```
5 2
1 5 3 2 4
C 1 5 1
Q 1 5
```

**输出**

```
20
```

## 题解

原数组：${a_1, a_2,a_3,....,a_n}$

前缀和数组：$S_i=\sum\limits_{k=1}^{k=i}{a_i}, a_i=S_i-S_{i-1}$

差分数组：$X_i=a_i-a_{i-1}$

$a$ 数组是 $S$ 数组的差分数组、$X$ 数组的前缀和数组

$Query(l, r) = S(r) - S(l - 1)$

$S_i= \sum\limits_{k=1}^{i}\sum\limits_{y=1}^{k}{X_y} = \sum\limits_{k=1}^{i}{(i + 1)X_k-k*X_k}=(i + 1)\sum\limits_{k=1}^{i}{X_k-\sum\limits_{k=1}^{i}k*X_k}$

设 $Y_i = i \times X_i, S_i=(i + 1)\sum\limits_{k=1}^{i}{X_k-\sum\limits_{k=1}^{i}{Y_k}}$，维护 $X, Y$ 两个序列的树状数组

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 100000
#define lowbit(x) (x & -x)

long long c[2][MAX_N + 5];

void add(long long k, long long i, long long x, long long n) {
    while (i <= n) {
        c[k][i] += x;
        i += lowbit(x);
    }
    return ;
}

long long query(long long k, long long i) {
    long long sum = 0;
    while (i) {
        sum += c[k][i];
        i -= lowbit(i);
    }
    return sum;
}

long long S(long long i) {
    return (i + 1) * query(0, i) - query(1, i);
}

void modify(long long i, long long x, long long n) {
    add(0, i, x, n);
    add(1, i, i * x, n);
    return ;
}

int main() {
    long long n, m;
    scanf("%lld%lld", &n, &m);
    for (long long i = 1, pre = 0, a; i <= n; ++i) {
        scanf("%lld", &a);
        // 维护差分值
        modify(i, a - pre, n);
        pre = a;
    }
    char s[10];
    for (long long i = 0, l, r, d; i < m; ++i) {
        scanf("%s", s);
        switch(s[0]) {
            case 'C':
                scanf("%lld%lld%lld", &l, &r, &d);
                // 对差分数组两次单点修改，作为对原数组的区间修改
                modify(l, d, n);
                modify(r + 1, -d, n);
                break;
            case 'Q':
                scanf("%lld%lld", &l, &r);
                printf("%lld\n", (S(r) - S(l - 1)));
                break;
        }
    }
    return 0;
}
```

