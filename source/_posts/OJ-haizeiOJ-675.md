---
title: OJ-haizeiOJ-675
date: 2021-03-06 19:25:41
mathjax: true
tag: [OJ, haizeiOJ, 最长路]
---

# [任务达人](http://oj.haizeix.com/problem/675)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210308145036564.png)

**样例输入**

```
4 10 3
1 2 3 4
1 2 5
2 4 2
3 4 4
```

**样例输出**

```
1
6
3
8
```

## 题解

就是 AOE 网求关键路径，套用最长路模板求即可。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 100000

struct edge {
    int e, v, next;
};

edge edg[MAX_N + 5];
int n, m, c, head[MAX_N + 5], in_degree[MAX_N + 5], ans[MAX_N + 5], edg_cnt;

void add_edg(int a, int b, int c) {
    edg[edg_cnt].e = b;
    edg[edg_cnt].v = c;
    edg[edg_cnt].next = head[a];
    head[a] = edg_cnt++;
    in_degree[b]++;
}

int main() {
    memset(head, -1, sizeof(head));
    // m 没用
    cin >> n >> m >> m;
    for (int i = 1; i <= n; ++i) {
        scanf("%d", ans + i);
    }
    for (int i = 1; i <= m; ++i) {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add_edg(a, b, c);
    }

    queue<int> que;
    for (int i = 0; i < n; ++i) {
        if (in_degree[i] == 0) {
            que.push(i);
        }
    }

    while (!que.empty()) {
        int temp = que.front();
        que.pop();
        for (int i = head[temp]; i != -1; i = edg[i].next) {
            int e = edg[i].e, v = edg[i].v;
            ans[e] = max(ans[e], ans[temp] + v);
            in_degree[e]--;
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



