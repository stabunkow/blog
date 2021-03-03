---
title: OJ-luogu-1462
date: 2021-03-02 10:25:41
tag: [OJ, luogu, 最短路算法, 二分搜索]
mathjax: true
---



# [通往奥格瑞玛的道路](https://www.luogu.com.cn/problem/P1462)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210302111246076.png)

**样例输入**

```
4 4 8
8
5
6
10
2 1 2
2 4 1
1 3 4
3 4 3
```

**样例输出**

```
10
```

## 题解

限制了可经过的单条最大路径长，然后搜索可行的最小值，属于二分搜索下（000111）搜索情况。然后再搜索的情况对每个限制值跑一次单源最短路径算法，看是否能满足题意要求。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 10000
#define MAX_M 50000
#define INF 0x3f

struct edge {
    int e, v, next;
};

int n, m, b, cost[MAX_N + 5];
edge edg[2 * MAX_M + 5]; // 双向路径
int edg_cnt, head[MAX_N + 5], ans[MAX_N + 5], mark[MAX_N + 5];

void add_edg(int a, int b, int c) {
    edg[edg_cnt].e = b;
    edg[edg_cnt].v = c;
    edg[edg_cnt].next = head[a];
    head[a] = edg_cnt++;
}

int func(int x) {
    memset(ans, 0x3f, sizeof(ans));
    memset(mark, 0, sizeof(mark));
	
    // 基于队列优化的 BF 算法
    queue<int> que;
    que.push(1);
    ans[1] = 0;
    while (!que.empty()) {
        int temp = que.front();
        que.pop();
        mark[temp] = 0;
        for (int i = head[temp]; i != -1; i = edg[i].next) {
            int e = edg[i].e, v = edg[i].v;
            if (ans[e] > ans[temp] + v && cost[e] <= x) {
                ans[e] = ans[temp] + v;
                if (mark[e] == 0) {
                    mark[e] = 1;
                    que.push(e);
                }
            }
        }
    }
    // 不能达到终点或者血量不足
    if (ans[n] != INF && ans[n] < b) {
        return 1;
    }
    return 0;
}

// 二分搜索
int binary_search(int l, int r) {
    while (l < r) {
        int mid = (l + r) / 2;
        if (func(mid)) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l;
}

int main() {
    memset(head, -1, sizeof(head));
    cin >> n >> m >> b;
    int r = 0;
    for (int i = 1; i <= n; ++i) {
        scanf("%d", cost + i);
        r = max(r, cost[i]);
    }
    for (int i = 1; i <= m; ++i) {
        int x, y, z;
        scanf("%d%d%d", &x, &y, &z);
        add_edg(x, y, z);
        add_edg(y, x, z);
    }

    int ansfin = binary_search(0, r);
    
    // 需要对最右侧值额外检测
    if (ansfin < r || func(r)) {
        cout << ansfin << endl;
    } else {
        cout << "AFK" << endl;
    }
    return 0;
}   
```

