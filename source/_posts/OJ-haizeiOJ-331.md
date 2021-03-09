---
title: OJ-haizeiOJ-331
date: 2021-03-09 18:28:41
mathjax: true
tag: [OJ, haizeiOJ, 树状数组, 二分搜索]
---

# [Lost_cows](http://oj.haizeix.com/problem/331)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210309164142740.png)

**输入**

```
5
1
2
1
0
```

**输出**

```
2
4
5
3
1
```

## 题解

利用树状数组单点修改为 1 表示该数出现过，例如位置 5 的开始搜索，发现其位置排名为 1，那么 1 被拿走，下标情况为 [0, 1, 1, 1, 1]，接着从位置 4 开始搜索，在 1 被拿走后排名为 2，那么根据树状数组 1 可用和值统计，位置 4 的数字为 3，以此类推。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 80000
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
int ind[MAX_N + 5];

void read() {
    scanf("%d", &n);
    ind[1] = 0;
    for (int i = 2; i <= n; ++i) {
        // 到 i 之前有多少个数字比其小
        scanf("%d", cnt + i);
    }
    for (int i = 1; i <= n; ++i) {
        // 利用树状数组性质标记每个数字可用
        add(i, 1, n);
    }
    return ;
}

// 二分搜索 000111
int binary_search(int n, int x) {
    int l = 1, r = n, mid;
    while (l < r) {
        mid = (l + r) >> 1;
        if (query(mid) < x) {
            l = mid + 1;
        } else {
            r = mid;
        }
    }
    return l;
}

void solve() {
    // 需要逆序
    for (int i = n; i >= 1; --i) {
        // 搜索本位置的数
        ind[i] = binary_search(n, cnt[i] + 1);
        // 改数字标记为不可用，并会影响再次排名的标记数量求和
        add(ind[i], -1, n);
    }
    return ;
}

void output() {
    for (int i = 1; i <= n; ++i) {
        printf("%d\n", ind[i]);
    }
    return ;
}

int main() {
    read();
    solve();
    output();
    return 0;
}
```

