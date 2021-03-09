---
title: OJ-haizeiOJ-224
date: 2021-03-09 16:26:41
mathjax: true
tag: [OJ, haizeiOJ, 线段树]
---

# [复合线段树](http://oj.haizeix.com/problem/224)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210309142708665.png)

**输入**

```
5 5 38
1 5 4 2 3
2 1 4 1
3 2 5
1 2 4 2
2 3 5 5
3 1 4
```

**输出**

```
17
2
```

## 题解

存在两种操作乘和加，可以使用两个懒标记，要注意两个标记会相互影响，初始化的值和对区间的操作影响不同。

```cpp
#include <bits/stdc++.h>
using namespace std;

#define MAX_N 100000
#define L(ind) (ind << 1)
#define R(ind) (ind << 1 | 1)

// 线段树模板
struct Node {
    // t1 乘标记
    // t2 加标记
    long long sum, t1, t2;
} tree[(MAX_N) << 2];

long long a[MAX_N + 5];
long long n, m, p;

// 加与乘不同处理
void mul_tag(long long ind, long long x) {
    tree[ind].sum *= x;
    tree[ind].sum %= p;
    tree[ind].t1 *= x;
    tree[ind].t1 %= p;
    tree[ind].t2 *= x;
    tree[ind].t2 %= p;
    return ;
}

void add_tag(long long ind, long long x, long long n) {
    tree[ind].sum += x * n;
    tree[ind].sum %= p;
    tree[ind].t2 += x;
    tree[ind].t2 %= p;
    return ;
}

void update(long long ind) {
    tree[ind].sum = tree[L(ind)].sum + tree[R(ind)].sum;
    tree[ind].sum %= p;
    return ;
}

void down(long long ind, long long l, long long r) {
    if (tree[ind].t1 - 1 || tree[ind].t2) {
        long long a = tree[ind].t1, b = tree[ind].t2;
        long long mid = (l + r) >> 1;
        mul_tag(L(ind), a);
        mul_tag(R(ind), a);
        add_tag(L(ind), b, mid - l + 1);
        add_tag(R(ind), b, r - mid);
        tree[ind].t1 = 1;
        tree[ind].t2 = 0;
    }
    return ;
}

void build_tree(long long ind, long long l, long long r) {
    tree[ind].t1 = 1;
    tree[ind].t2 = 0;
    if (l == r) {
        tree[ind].sum = a[l];
        return ;
    }
    long long mid = (l + r) >> 1;
    build_tree(L(ind), l, mid);
    build_tree(R(ind), mid + 1, r);
    update(ind);

    return ;
}

void modify(long long ind, long long l, long long r, long long flag, long long x, long long y, long long val) {
    if (x <= l && r <= y) {
        if (flag == 0) {
            mul_tag(ind, val);
        } else {
            add_tag(ind, val, r - l + 1);
        }
        return ;
    }
    
    long long mid = (l + r) >> 1;
    down(ind, l, r);
    if (x <= mid) {
        modify(L(ind), l, mid, flag, x, y, val);
    }
    if (mid < y) {
        modify(R(ind), mid + 1, r, flag, x, y, val);
    }
    update(ind);
    return ;
}

long long query(long long ind, long long l, long long r, long long x, long long y) {
    if (x <= l && r <= y) {
        return tree[ind].sum;
    }

    long long mid = (l + r) >> 1;
    long long ans = 0;
    down(ind, l, r);
    if (x <= mid) {
        ans += query(L(ind), l, mid, x, y);
        ans %= p;
    }
    if (mid < y) {
        ans += query(R(ind), mid + 1, r, x, y);
        ans %= p;
    }

    update(ind);
    return ans;
}

int main() {
    scanf("%lld%lld%lld", &n, &m, &p);
    for (long long i = 1; i <= n; ++i) {
        scanf("%lld", a + i);
    }
    build_tree(1, 1, n);
    long long op, x, y, k;
    for (long long i = 0; i < m; ++i) {
        scanf("%lld%lld%lld", &op, &x, &y);
        switch (op) {
            case 1:
            case 2:
                scanf("%lld", &k);
                modify(1, 1, n, op - 1, x, y, k);
                break;
            case 3:
                printf("%lld\n", query(1, 1, n, x, y));
                break;
        }
    }

    return 0;
}
```

