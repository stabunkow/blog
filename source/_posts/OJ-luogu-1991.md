---
title: OJ-luogu-1991
date: 2021-03-03 09:27:41
tag: [OJ, luogu, 最小生成树]
mathjax: true
---



# [无线通讯网](https://www.luogu.com.cn/problem/P1991)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210303103559632.png)

**样例输入**

```
2 4
0 100
0 300
0 600
150 750
```

**样例输出**

```
212.13
```

## 题解

题意是求最小生成树生成过程中，$p$ 个哨所连通过程中，后 $s$ 个卫星站点用 $s-1$ 条边相连，之前站点间的连接的关系需要用无线电，并把之前连接最后一次的距离值作为无线点最大连接距离，所以要用 Kruskal 算法。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 500

struct edge {
    int s, e;
    double v;
    bool operator<(const edge &b) const {
        return this->v < b.v;
    }
};

int s, p, xy[MAX_N + 5][2], my_union[MAX_N + 5], edg_cnt, cnt;
edge edg[MAX_N * MAX_N + 5];
double ans;

int find_fa(int x) {
    if (my_union[x] == x) {
        return x;
    }
    return my_union[x] = find_fa(my_union[x]);
}


int main() {
    cin >> s >> p;
    for (int i = 1; i <= p; ++i) {
        cin >> xy[i][0] >> xy[i][1];
        my_union[i] = i;
    }
    for (int i = 1; i <= p; ++i) {
        for (int j = i + 1; j <= p; ++j) {
            int t1 = xy[i][0] - xy[j][0], t2 = xy[i][1] - xy[j][1];
            double t = sqrt(t1 * t1 + t2 * t2);
            edg[edg_cnt].s = i;
            edg[edg_cnt].e = j;
            edg[edg_cnt].v = t;
            edg_cnt++;
        }
    }
    sort(edg, edg + edg_cnt);
    for (int i = 0; i < edg_cnt; ++i) {
        int fa = find_fa(edg[i].s), fb = find_fa(edg[i].e);
        if (fa != fb) {
            ans = edg[i].v;
            my_union[fa] = fb;
            cnt++;
            // p - 1 - (s - 1)
            if (cnt == p - s) break;
        }
    }
    printf("%.2f\n", ans);
    return 0;
}
```

