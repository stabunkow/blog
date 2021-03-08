---
title: OJ-luogu-4047
date: 2021-03-04 13:29:41
tag: [OJ, luogu, 最小生成树]
mathjax: true
---



# [部落划分](https://www.luogu.com.cn/problem/P4047)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210304154346034.png)

**样例输入**

```
9 3
2 2
2 3
3 2
3 3
3 5
3 6
4 6
6 2
6 3
```

**样例输出**

```
2.00
```

## 题解

本题可以理解为求最小生成树生成过程中，加入图中的第 $n - 1 - (k - 2)$ 长的边就是题目所求结果，由于题目点数量不多，可以计算所有边的距离然后用 Kruskal 算法求解。 

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

