---
title: OJ-haizeiOJ-635
date: 2021-03-04 19:51:41
mathjax: true
tag: [OJ, haizeiOJ, 拓扑排序]
---

# [神经网络](http://oj.haizeix.com/problem/635)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210304211442786.png)

**样例输入**

```
5 6
1 0
1 0
0 1
0 1
0 1
1 3 1
1 4 1
1 5 1
2 3 1
2 4 1
2 5 1
```

**样例输出**

```
3 1
4 1
5 1
```

## 题解

同时需要记录出度，确定哪些是神经网络终点。根据题意，在拓扑排序的过程中，节点往节点间传值。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 100
#define MAX_M 10000

struct edge {
    int e, v, next;
};

int n, m, c[MAX_N + 5], u[MAX_N + 5], head[MAX_N + 5], in_degree[MAX_N + 5], out_degree[MAX_N + 5], edg_cnt;
edge edg[MAX_M + 5];

void add_edg(int a, int b, int c) {
    edg[edg_cnt].e = b;
    edg[edg_cnt].v = c;
    edg[edg_cnt].next = head[a];
    head[a] = edg_cnt++;
    in_degree[b]++;
    out_degree[a]++;
}

int main() {
    memset(head, -1, sizeof(head));
    cin >> n >> m;
    queue<int> que;
    for (int i = 1; i <= n; ++i) {
        cin >> c[i] >> u[i];
        // 表示为输入单元
        if (c[i] != 0) {
            que.push(i);
        }
    }
    // 本题需要考虑边的权值
    for (int i = 0; i < m; ++i) {
        int a, b, c;
        cin >> a >> b >> c;
        add_edg(a, b, c);
    }

    while (!que.empty()) {
        int t = que.front();
        que.pop();
        for (int i = head[t]; i != -1; i = edg[i].next) {
            int e = edg[i].e, v = edg[i].v;
            if (c[t] > 0) {
                c[e] += c[t] * v;
            }
            in_degree[e]--;
            if (in_degree[e] == 0) {
                que.push(e);
                c[e] -= u[e];
            }
        }
    }
    int f = 0;
    for (int i = 1; i <= n; ++i) {
        if (out_degree[i] == 0 && c[i] > 0) {
            cout << i << " " << c[i] << endl;
            f = 1;
        }
    }
    if (f == 0) {
        cout << "NULL" << endl;
    }

    return 0;
}
```

