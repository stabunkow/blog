---
title: OJ-haizeiOJ-636
date: 2021-03-04 19:53:41
mathjax: true
tag: [OJ, haizeiOJ, 拓扑排序]
---

# [旅行计划](http://oj.haizeix.com/problem/636)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210304211958184.png)

**样例输入**

```
5 6
1 2
1 3
2 3
2 4
3 4
2 5
```

**样例输出**

```
1
2
3
4
3
```

## 题解

根据题意，在拓扑排序的过程中，通过节点与节点间数值传递，记录拓扑排序起点到终点的最大长度。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 100000
#define MAX_M 200000

struct edge {
    int e, next;
};

int n, m, head[MAX_N + 5], edg_cnt, ans[MAX_M + 5], in_degree[MAX_N + 5];
edge edg[MAX_M + 5];

void add_edg(int a, int b) {
    edg[edg_cnt].e = b;
    edg[edg_cnt].next = head[a];
    head[a] = edg_cnt++;
    in_degree[b]++;
}

int main() {
    memset(head, -1, sizeof(head));
    
    cin >> n >> m;
    for (int i = 0; i < m; ++i) {
        int a, b;
        scanf("%d%d", &a, &b);
        add_edg(a, b);
    }

    queue<int> que;
    for (int i = 1; i <= n; ++i) {
        if (in_degree[i] == 0) {
            que.push(i);
            ans[i] = 1;
        }
    }

    while (!que.empty()) {
        int t = que.front();
        que.pop();
        for (int i = head[t]; i != -1; i = edg[i].next) {
            int e = edg[i].e;
            in_degree[e]--;
            ans[e] = max(ans[e], ans[t] + 1);
            if (in_degree[e] == 0) {
                que.push(e);
            }
        }
    }

    for (int i = 1; i <= n; ++i) {
        printf("%d\n", ans[i]);
    }

    return 0;
}
```

