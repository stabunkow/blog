---
title: OJ-luogu-1629
date: 2021-03-01 21:25:41
tag: [OJ, luogu, 最短路算法]
mathjax: true
---



# [邮递员送信](https://www.luogu.com.cn/problem/P1629)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210301210712728.png)

**样例输入**

```
5 10
2 3 5
1 5 5
3 5 6
1 2 8
1 3 8
5 3 4
4 1 8
4 5 3
3 5 6
5 4 2
```

**样例输出**

```
83
```

## 题解

本题可以将一张图分解为来去两个不同方向的图，对两个图求来去两个方向到目的地的单源最短路径，然后根据题意求最小和。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 100000

struct edge {
    int e, v, next;
};

edge edg[2][MAX_N + 5];
int n, m, head[2][MAX_N + 5], ans[2][MAX_N + 5], edg_cnt[2];
int mark[MAX_N + 5], ansfin;

// 可使用任意单源最短路算法
// 使用队列优化的 BF 算法
void func(edge *edg, int *head, int *ans) {
    queue<int> que;
    que.push(1);
    while (!que.empty()) {
        int temp = que.front();
        que.pop();
        mark[temp] = 0;
        for (int i = head[temp]; i != -1; i = edg[i].next) {
            int e = edg[i].e, v = edg[i].v;
            if (ans[e] > ans[temp] + v) {
                ans[e] = ans[temp] + v;
                if (mark[e] == 0) {
                    que.push(e);
                    mark[e] = 1;
                }
            }
        }
    }
}

void add_edg(edge *edg, int *head, int &edg_cnt, int s, int e, int v) {
    edg[edg_cnt].e = e;
    edg[edg_cnt].v = v;
    edg[edg_cnt].next = head[s];
    head[s] = edg_cnt++;
}

int main() {
    for (int i = 0; i < 2; ++i) {
        memset(ans[i], 0x3f, sizeof(ans[i]));
        memset(head[i], -1, sizeof(head[i]));
    }
    cin >> n >> m;
    for (int i = 0; i < m; ++i) {
        int a, b, c;
        cin >> a >> b >> c;
        add_edg(edg[0], head[0], edg_cnt[0], a, b, c);
        add_edg(edg[1], head[1], edg_cnt[1], b, a, c);
    }
    ans[0][1] = ans[1][1] = 0;
    func(edg[0], head[0], ans[0]);
    func(edg[1], head[1], ans[1]);
    for (int i = 1; i <= n; ++i) {
        ansfin += ans[0][i] + ans[1][i];
    }
    cout << ansfin << endl;
    return 0;
}
```

