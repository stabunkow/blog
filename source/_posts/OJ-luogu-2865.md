---
title: OJ-luogu-2865
date: 2021-03-02 09:25:41
tag: [OJ, luogu, 最短路算法]
mathjax: true
---



# [Roadblocks G](https://www.luogu.com.cn/problem/P2865)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210302095937379.png)

**样例输入**

```
4 4
1 2 100
2 4 200
2 3 250
3 4 100
```

**样例输出**

```
450
```

## 题解

求的是次短路径长，可以同时维护最短路径长数组和次短路径长数组，搞清以下逻辑：最短路可以被最短路更新，最短路更新时，次短路就变成原来的最短路；次短路可以被最短路更新，但是要严格保证次短路径与最短路径不同；次短路可以被次短路更新。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 100000

struct edge {
    int e, v, next;
};

edge edg[MAX_N * 2 + 5];
int n, m, edg_cnt, head[MAX_N + 5], mark[MAX_N + 5];
// 最短路，次短路
int ans[MAX_N + 5], ans2[MAX_N + 5];

void add_edg(int a, int b, int c) {
    edg[edg_cnt].e = b;
    edg[edg_cnt].v = c;
    edg[edg_cnt].next = head[a];
    head[a] = edg_cnt++;
}

int main() {
    memset(ans, 0x3f, sizeof(ans));
    memset(ans2, 0x3f, sizeof(ans2));
    memset(head, -1, sizeof(head));

    cin >> n >> m;
    for (int i = 0; i < m; ++i) {
        int a, b, c;
        cin >> a >> b >> c;
        // 确定起点次短路的长度
        if (a == 1 || b == 1) {
            ans2[1] = min(ans2[1], 2 * c);
        }
        add_edg(a, b, c);
        add_edg(b, a, c);
    }

    queue<int> que;
    que.push(1);
    ans[1] = 0;
    while (!que.empty()) {
        int temp = que.front();
        que.pop();
        mark[temp] = 0;
        for (int i = head[temp]; i != -1; i = edg[i].next) {
            int e = edg[i].e, v = edg[i].v;
            // 通过最短路更新最短路
            if (ans[e] > ans[temp] + v) {
                ans2[e] = ans[e];
                ans[e] = ans[temp] + v;
                if (mark[e] == 0) {
                    mark[e] = 1;
                    que.push(e);
                }
            }
            // 通过最短路更新次短路，严格限定最短路不能和次短路一样 
            if (ans2[e] > ans[temp] + v && ans[e] != ans[temp] + v) {
                ans2[e] = ans[temp] + v;
                if (mark[e] == 0) {
                    mark[e] = 1;
                    que.push(e);
                }
            }
            // 通过次短路更新次短路
            if (ans2[e] > ans2[temp] + v) {
                ans2[e] = ans2[temp] + v;
                if (mark[e] == 0) {
                    mark[e] = 1;
                    que.push(e);
                }
            }
        }
    }

    cout << ans2[n] << endl;

    return 0;
}
```

