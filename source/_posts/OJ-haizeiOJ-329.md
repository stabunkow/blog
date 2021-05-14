---
title: OJ-haizeiOJ-329
date: 2021-03-09 18:26:41
mathjax: true
tag: [OJ, haizeiOJ, 树状数组]
---

# [弱化的整数问题](http://oj.haizeix.com/problem/329)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210309161914457.png)

**输入**

```
5
1 5 3 2 4
2
C 1 5 1
Q 3
```

**输出**

```
4
```

## 题解

原数组：${a_1, a_2,a_3,....,a_n}$

差分数组：$X_i=a_i-a_{i-1}$

$a$ 数组是 $X$ 数组的前缀和数组

所以用树状数组维护差分数组，那么 $query$ 求的即为原元素。输入时需要维护差分值，对差分数组两次单点修改，作为对原数组的区间修改。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 100000
#define lowbit(x) (x & -x)

// 树状树组模板
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

int main() {
    int m, n, a, pre = 0;
    scanf("%d", &n);
    for (int i = 1; i <= n; ++i) {
        scanf("%d", &a);
        add(i, a - pre, n); // 维护差分值
        pre = a;
    }
    scanf("%d", &m);
    char s[10];
    int l, r, x;
    while (m--) {
        scanf("%s", s);
        switch (s[0]) {
            case 'C':
                scanf("%d%d%d", &l, &r, &x);
                // 修改两次
                add(l, x, n);
                add(r + 1, -x, n);
                break;
            case 'Q':
                scanf("%d", &x);
                printf("%d\n", query(x));
                break;
        }
    }
    return 0;
}

```

