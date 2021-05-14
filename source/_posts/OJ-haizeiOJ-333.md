---
title: OJ-haizeiOJ-333
date: 2021-03-09 16:27:41
mathjax: true
tag: [OJ, haizeiOJ, 线段树]
---

# [区间最大子段和](http://oj.haizeix.com/problem/333)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210309143749922.png)

**输入**

```
5 3
1 2 -3 4 5
1 2 3
2 2 -1
1 3 2
```

**输出**

```
2
-1
```

## 题解

区间最大子段和，为左子区间的最大子段和、右区间的最大子段和，左子区间连续右最大字段和与右子区间连续左子段和之和 三者值中的最大值，维护这些值本身需要区间和信息，所以一共要维护 4 个子段。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 500000
#define L(ind) (ind << 1)
#define R(ind) (ind << 1 | 1)
#define SUM(ind) tree[ind].sum
#define INMAX(ind) tree[ind].inmax
#define LMAX(ind) tree[ind].lmax
#define RMAX(ind) tree[ind].rmax

// 线段树模板
// 区间最大字段和要考虑左右分区情况
struct Data {
    int sum, inmax, lmax, rmax;
} _tree[MAX_N << 2];
Data *tree = _tree + 1;

int n, m, flag;
int val[MAX_N + 5];

// 用途1：向上更新
// 用途2：在 query 中
void UP(int a, int b, int c) {
    SUM(a) = SUM(b) + SUM(c);
    LMAX(a) = max(LMAX(b), SUM(b) + LMAX(c));
    RMAX(a) = max(RMAX(c), SUM(c) + RMAX(b));
    INMAX(a) = max(max(INMAX(b), INMAX(c)), RMAX(b) + LMAX(c));
    return ;
}

void build_tree(int ind, int l, int r) {
    if (l == r) {
        SUM(ind) = INMAX(ind) = LMAX(ind) = RMAX(ind) = val[l];
		return ;
    }
    int mid = (l + r) >> 1;
    build_tree(L(ind), l, mid);
    build_tree(R(ind), mid + 1, r);
    UP(ind, L(ind), R(ind));

    return ;
}

void modify(int ind, int l, int r, int i, int x) {
    if (l == r) {
        SUM(ind) = INMAX(ind) = LMAX(ind) = RMAX(ind) = x;
        return ;
    }
    int mid = (l + r) >> 1;
    if (i <= mid) {
        modify(L(ind), l, mid, i, x);
    } else {
        modify(R(ind), mid + 1, r, i, x);
    }
    UP(ind, L(ind), R(ind));
    return ;
}

// 值可能会被多次更新，使用全局变量返回查询，最终从 tree[0] 返回
void query(int ind, int l, int r, int x, int y) {
    if (x <= l && r <= y) {
        if (flag) {
            tree[0] = tree[ind];
            flag = 0;
        } else {
            // 左侧先被更新，已更新的值再和右侧结合更新
            UP(-1, 0, ind);
            tree[0] = tree[-1];
        }
        return ;
    }

    int mid = (l + r) >> 1;
    if (x <= mid) {
        query(L(ind), l, mid, x, y);
    }
    if (mid < y) {
        query(R(ind), mid + 1, r, x, y);
    }
    return ;
}

int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; ++i) {
        scanf("%d", val + i);
    }
    build_tree(1, 1, n);
    while (m--) {
        int k, x, y;
        scanf("%d%d%d", &k, &x, &y);
        switch (k) {
            case 1:
                if (x > y) swap(x, y);
                flag = 1;
                query(1, 1, n, x, y);
                printf("%d\n", INMAX(0));
                break;
            case 2:
                modify(1, 1, n, x, y);
                break;
        }
    }
    return 0;
}
```

