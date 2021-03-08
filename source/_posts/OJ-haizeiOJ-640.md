---
title: OJ-haizeiOJ-640
date: 2021-03-04 19:50:41
mathjax: true
tag: [OJ, haizeiOJ, 拓扑排序]
---

# [食物链计数](http://oj.haizeix.com/problem/640)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210304195154077.png)

**样例输入**

```
7 9
1 3
1 4
2 3
2 4
2 5
3 6
4 6
4 7
5 7
```

**样例输出**

```
7
```

## 题解

同时需要记录出度，确定哪些是食物链终点。中间点上之前的食物链数量和为到达该点之前的食物链之和，累加向后传递即可。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 5000
#define MAX_M 500000
#define P 100000007

struct edge {
    int e, next;
};

int n, m, head[MAX_N + 5], edg_cnt, ans[MAX_M + 5], ansfin, in_degree[MAX_N + 5], out_degree[MAX_N + 5];
edge edg[MAX_M + 5];

void add_edg(int a, int b) {
    edg[edg_cnt].e = b;
    edg[edg_cnt].next = head[a];
    head[a] = edg_cnt++;
    in_degree[b]++;
    out_degree[a]++;
}

int main() {
    memset(head, -1, sizeof(head));
    
    cin >> n >> m;
    for (int i = 0; i < m; ++i) {
        int a, b;
        cin >> a >> b;
        add_edg(a, b);
    }

    queue<int> que;
    for (int i = 1; i <= n; ++i) {
        if (in_degree[i] == 0) {
            que.push(i);
            // 食物链起点数量默认 1
            ans[i] = 1;
        }
    }

    while (!que.empty()) {
        int t = que.front();
        que.pop();
        for (int i = head[t]; i != -1; i = edg[i].next) {
            int e = edg[i].e;
            in_degree[e]--;
            ans[e] += ans[t];
            ans[e] %= P;
            // 进行下一次拓扑序
            if (in_degree[e] == 0) {
                que.push(e);
            }
        }
    }

    for (int i = 1; i <= n; ++i) {
        if (out_degree[i] == 0) {
            ansfin += ans[i];
            ansfin %= P;
        }
    }

    cout << ansfin << endl;

    return 0;
}
```

