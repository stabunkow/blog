---
title: OJ-luogu-4826
date: 2021-03-06 21:25:41
mathjax: true
tag: [OJ, luogu, 最短路]
---

# [玛丽卡](https://www.luogu.com.cn/problem/P1186)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210308200533687.png)

**样例输入**

```
5 7
1 2 8
1 4 10
2 3 9
2 4 10
2 5 1
3 4 7
3 5 10
```

**样例输出**

```
27
```

## 题解

先求出默认的最短路径，然后最短路径中可能存在某一条边堵车，对原最短路径上所有可能的边进行排查，假设该边堵车，设置为不可达再跑一遍最短路，记录修改后以新的最短路到达的时间，记录最大值。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 1000
#define INF 0x3f3f3f3f

struct edge {
    int e, v, next;
};

struct node {
    int now, val;
    bool operator<(const node &b) const {
        return this->val > b.val;
    }
};

edge edg[MAX_N * MAX_N + 5];
int n, m, head[MAX_N + 5], ans[MAX_N + 5], fin, from[MAX_N + 5], edg_cnt;
int cut[MAX_N + 5][MAX_N + 5];

void add_edg(int a, int b, int c) {
    edg[edg_cnt].e = b;
    edg[edg_cnt].v = c;
    edg[edg_cnt].next = head[a];
    head[a] = edg_cnt++;
}

// f 记录路径
void func(int f) {
    int mark[MAX_N + 5] = {0};
    memset(ans, 0x3f, sizeof(ans));

    // BF 基于队列的优化算法
    queue<int> que;
    que.push(1);
    ans[1] = 0;
    while (!que.empty()) {
        int temp = que.front();
        que.pop();
        mark[temp] = 0;
        for (int i = head[temp]; i != -1; i = edg[i].next) {
            int e = edg[i].e, v = edg[i].v;
            if (cut[temp][e] == 0 && ans[e] > ans[temp] + v) {
                ans[e] = ans[temp] + v;
                if (f == 1) from[e] = temp;
                if (mark[e] == 0) {
                    mark[e] = 1;
                    que.push(e);
                }
            }
        }
    }
}


int main() {
    memset(head, -1, sizeof(head));
    cin >> n >> m;

    for (int i = 0; i < m; ++i) {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add_edg(a, b, c);
        add_edg(b, a, c);
    }

    func(1);
    fin = ans[n];
    for (int i = n; i != 1; i = from[i]) {
        // i -> from[i]

        // 跑前和跑后需要复原路径
        cut[i][from[i]] = cut[from[i]][i] = 1;
        func(0);
        fin = max(fin, ans[n]);
        cut[i][from[i]] = cut[from[i]][i] = 0;
    }

    cout << fin << endl;
 
    return 0;
}
```
