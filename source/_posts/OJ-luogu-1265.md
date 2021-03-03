---
title: OJ-luogu-1265
date: 2021-03-03 09:25:41
tag: [OJ, luogu, 最小生成树]
mathjax: true
---



# [公路修建](https://www.luogu.com.cn/problem/P1265)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210303101236325.png)

**样例输入**

```
4
0 0
1 2
-1 2
0 4
```

**样例输出**

```
6.47
```

## 题解

求点与点之间生成的最小生成树，本题没有默认给出线（城市与城市间的距离），需要自行计算，若使用 Kruskal 算法存储边空间不足，只能用 Prim 算法遍历计算到每个边的距离。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 5000

struct node {
    int now;
    double v;
    bool operator< (const node &b) const {
        return this->v > b.v;
    }
};

int n, xy[MAX_N + 5][2], mark[MAX_N + 5], cnt;
double ans, num[MAX_N + 5];

// 计算点与点之间的距离
double func(int a, int b) {
    double t1 = xy[a][0] - xy[b][0], t2 = xy[a][1] - xy[b][1];
    return sqrt(t1 * t1 + t2 * t2);
}

int main() {
    cin >> n;
    for (int i = 1; i <= n; ++i) {
        cin >> xy[i][0] >> xy[i][1];
        num[i] = 1e9;
    }
    // 使用最小堆一定是最短边先出
    priority_queue<node> que;
    que.push((node) {1, 0});
    num[1] = 0;
    while (!que.empty()) {
        node temp = que.top();
        que.pop();
        if (mark[temp.now] == 1) {
            continue;
        }
        mark[temp.now] = 1;
        ans += temp.v;
        cnt++;
        if (cnt == n) break;
        for (int i = 1; i <= n; ++i) {
            if (mark[i] == 0) {
                double v = func(temp.now, i);
                // num[i] 表示点 i 到其他边的最短距离
                // 此题边数较多，这样可以限制推入堆的数量（反正也是最短边先出）
                if (v < num[i]) {
                    num[i] = v;
                    que.push((node) {i, v});
                }
            }
        }
    }
    printf("%.2f\n", ans);
    return 0;
}
```

