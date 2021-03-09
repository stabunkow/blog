---
title: OJ-haizeiOJ-332
date: 2021-03-09 18:30:41
mathjax: true
tag: [OJ, haizeiOJ, 树状数组, 二分搜索]
---

# [买票](http://oj.haizeix.com/problem/332)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210309165915148.png)

**输入**

```
4
0 77
1 51
1 33
2 69
```

**输出**

```
77 33 69 51
```

## 题解

先把所有位置视为数字，标记为 1 可用，利用 二分搜索 树状数组求和值作为排名找到位置数，然后把位置拥有的标记去掉，再次 二分搜索 时该数字不再记为排名统计，要逆序操作保证 二分搜索时 不会影响。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 200000
#define lowbit(x) (x & -x)

// 树状数组模板
int c[MAX_N + 5];

void add(int i, int x, int n) {
    while (i <= n) {
        c[i] += x;
        i += lowbit(i);
    }
    return ;
}

int query(int i) {
    int sum = 0;
    while (i) {
        sum += c[i];
        i -= lowbit(i);
    }
    return sum;
}

int n;
int cnt[MAX_N + 5];
int val[MAX_N + 5];
int ans[MAX_N + 5];

void read() {
    scanf("%d", &n);
    for (int i = 1; i <= n; ++i) {
        scanf("%d%d", cnt + i, val + i);
        // 利用树状数组性质标记每个数字（位置）可用
        add(i, 1, n);
    }
    return ;
}

// 找到可用的数字（位置） 00001111
int binary_search(int n, int x) {
    int l = 1, r = n, mid;
    while (l < r) {
        mid = (l + r) >> 1;
        if (query(mid) < x) l = mid + 1;
        else r = mid;
    }
    return l;
}

void solve() {
    for (int i = n; i >= 1; --i) {
        int num = binary_search(n, cnt[i] + 1);
        add(num, -1, n);
        ans[num] = val[i]; // 该位置的值
    }
    return ;
}

void output() {
    for (int i = 1; i <= n; ++i) {
        i == 1 || printf(" ");
        printf("%d", ans[i]);
    }
    printf("\n");
    return ;
}

int main() {
    read();
    solve();
    output();
    return 0;
}
```

