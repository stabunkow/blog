---
title: OJ-luogu-1144
date: 2021-03-01 21:27:41
tag: [OJ, luogu, 最短路算法]
mathjax: true
---



# [最短路计数](https://www.luogu.com.cn/problem/P1144)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210301212105070.png)

**样例输入**

```
5 7
1 2
1 3
2 4
3 4
2 3
4 5
4 5
```

**样例输出**

```
1
1
1
2
4
```

## 题解

本题求单源最短路径，需要注意记录最短路径的一些细节。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 1000000
#define MAX_M 4000000
#define P 100003

struct edge {
    int e, v, next;
};

struct node {
    int num, dis;
    bool operator< (const node &b) const {
        return this->dis > b.dis;
    }
};

edge edg[MAX_N + 5];
int n, m, edg_cnt, head[MAX_N + 5], ans[MAX_N + 5], ans_cnt[MAX_N + 5];

void add_edg(int a, int b) {
    edg[edg_cnt].e = b;
    edg[edg_cnt].v = 1;
    edg[edg_cnt].next = head[a];
    head[a] = edg_cnt++;
}

int main() {
    memset(ans, 0x3f, sizeof(ans));
    memset(head, -1, sizeof(head));
    cin >> n >> m;
    for (int i = 0; i < m; ++i) {
        int a, b;
        cin >> a >> b;
        add_edg(a, b);
        add_edg(b, a);
    }
    
    priority_queue<node> que;
    que.push((node) {1, 0});
    ans[1] = 0;
    ans_cnt[1] = 1;
    while (!que.empty()) {
        node temp = que.top();
        que.pop();
        if (temp.dis > ans[temp.num]) continue;
        for (int i = head[temp.num]; i != -1; i = edg[i].next) {
            int e = edg[i].e;
            // 最短路径值被更新
            if (ans[e] > ans[temp.num] + 1) {
                ans[e] = ans[temp.num] + 1;
                ans_cnt[e] = ans_cnt[temp.num];
                ans_cnt[e] %= P;
                que.push((node){e, ans[e]});
            } else if (ans[e] == ans[temp.num] + 1) {
                // 存在经过其他点的最小路径
                ans_cnt[e] += ans_cnt[temp.num];
                ans_cnt[e] %= P;
            }
        }
    }
    for (int i = 1; i <= n; ++i) {
        cout << ans_cnt[i] << endl;
    }

    return 0;
}
```

