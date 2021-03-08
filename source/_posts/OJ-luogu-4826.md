---
title: OJ-luogu-4826
date: 2021-03-06 20:25:41
mathjax: true
tag: [OJ, luogu, 最小生成树]
---

# [Superbull S](https://www.luogu.com.cn/problem/P4826)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210308194623814.png)

**样例输入**

```
4
3
6
9
10
```

**样例输出**

```
37
```

## 题解

两两关系连接，且要总得分最高。可以把两支队伍比赛的关系看出边，得分为边上权值。要求得的即为 最大生成树，形成连通图且边权值之和最高，可用 Kruskal 算法求解该问题。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 2000

struct edge {
    long long s, e, v;
    bool operator<(const edge &b) const {
        return this->v > b.v;
    }
};

int my_union[MAX_N + 5], num[MAX_N + 5];
long long n, m, ans, now;
edge edg[MAX_N * MAX_N + 5];

int find_fa(int x) {
    if (my_union[x] == x) {
        return x;
    }
    return my_union[x] = find_fa(my_union[x]);
}

int main() {
    cin >> n;

    for (int i = 1; i <= n; ++i) {
        cin >> num[i];
        my_union[i] = i;
    }

    for (int i = 1; i <= n; ++i) {
        for (int j = i + 1; j <= n; ++j) {
            edg[m].s = i;
            edg[m].e = j;
            edg[m].v = num[i] ^ num[j];
            m++;
        }
    }
    // m 为边的数量
    sort(edg, edg + m);
    for (int i = 0; i < m; ++i) {
        int fa = find_fa(edg[i].s), fb = find_fa(edg[i].e);
        if (fa != fb) {
            ans += edg[i].v;
            my_union[fa] = fb;
            now++;
            if (now == n) break;
        }
    }

    cout << ans << endl;

    return 0;
}
```



