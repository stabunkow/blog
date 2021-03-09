---
title: OJ-haizeiOJ-328
date: 2021-03-09 18:31:41
mathjax: true
tag: [OJ, haizeiOJ, 树状数组]
---

# [楼兰图腾](http://oj.haizeix.com/problem/328)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210309170601118.png)

**输入**

```
5
1 5 3 2 4
```

**输出**

```
3 4
```

## 题解

利用树状数组单点修改为 1 表示该数出现过，求和能统计前面有多少数字出现。例如插入 1 标记过后，$query(3)$ 能发现有 1 个数字会比其小，再利用该数值发现其他数量关系。

```cpp
#include <bits/stdc++.h>
using namespace std;

#define MAX_N 200000
#define lowbit(x) (x & -x)

long long c[MAX_N + 5];

void add(long long i, long long x, long long n) {
    while (i <= n) {
        c[i] += x;
        i += lowbit(i);
    }
    return ;
}

long long query(long long i) {
    long long sum = 0;
    while (i) {
        sum += c[i];
        i -= lowbit(i);
    }
    return sum;
}

long long n;
long long val[MAX_N + 5];

void read() {
    scanf("%lld", &n);
    for (long long i = 1; i <= n; ++i) {
        scanf("%lld", val + i);
    }
    return ;
}

void solve(long long &x, long long &y) {
    x = y = 0;
    for (long long i = 1; i <= n; ++i) {
        // 例如输入 1 3 5 2 4 时， 3 查询时可知前有 1 比其小，后只有 2 比其小 
        // 前面小于当前元素的数量
        long long a1 = query(val[i]);
        // 后面小于当前元素的数量
        long long a2 = val[i] - a1 - 1;
        // 前面大于当前元素的数量
        long long b1 = i - a1 - 1;
        // 后面大于当前元素的数量
        long long b2 = n - val[i] - b1;
        x += b1 * b2;
        y += a1 * a2;
        // 标记该数出现过
        add(val[i], 1, n);
    }
    return ;
}

int main() {
    read();
    long long a, b;
    solve(a, b);
    printf("%d %d\n", a, b);
    return 0;
}
```

