---
title: OJ-luogu-2820
date: 2021-03-03 19:29:41
tag: [OJ, luogu, 最小生成树]
mathjax: true
---



# [局域网](https://www.luogu.com.cn/problem/P2820)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210303201902638.png)

**样例输入**

```
5 5
1 2 8
1 3 1
1 5 3
2 4 5
3 4 2
```

**样例输出**

```
8
```

## 题解

求的就是所有边的和减去最小生成树的值，用 Kruskal 算法和 Prim 算法均可。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 100
#define MAX_M 1000

struct edge {
    int s, e, v;
    bool operator< (const edge &b) const {
        return this->v < b.v;
    }
};

edge edg[MAX_M + 5];
int n, k, my_union[MAX_N + 5], ans, cnt, total;

int find_fa(int x) {
    if (my_union[x] == x) {
        return x;
    }
    return my_union[x] = find_fa(my_union[x]);
}

int main() {
    cin >> n >> k;
    for (int i = 1; i <= n; ++i) {
        my_union[i] = i;
    }
    for (int i = 0; i < k; ++i) {
        cin >> edg[i].s >> edg[i].e >> edg[i].v;
        total += edg[i].v;
    }
    sort(edg, edg + k);
    for (int i = 0; i < k; ++i) {
        int fa = find_fa(edg[i].s), fb = find_fa(edg[i].e);
        if (fa != fb) {
            cnt++;
            my_union[fa] = fb;
            ans += edg[i].v;
            if (cnt == n - 1) break;
        }
    }
    cout << (total - ans) << endl;
    return 0;
}
```

