---
title: OJ-luogu-2504
date: 2021-03-03 20:29:41
tag: [OJ, luogu, 最小生成树]
mathjax: true
---



# [聪明的猴子](https://www.luogu.com.cn/problem/P2504)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210303210911111.png)

**样例输入**

```
4
1 2 3 4
6
0 0
1 0
1 2
-1 -1
-2 0
2 2
```

**样例输出**

```
3
```

## 题解

求的就是最小生成树的生成过程中加入边的最大值，本题数据量小，可计算存完边用 Kruskal 算法加入的最后一条边即为最大距离，或 Prim 算法遍历计算保存最大边值。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 1000
#define MAX_M 500

struct node {
    int now;
    double v;
    bool operator< (const node &b) const {
        return this->v > b.v;
    }
};

int mark[MAX_N + 5];
int m, n, monkey[MAX_M + 5], xy[MAX_N + 5][2], cnt;
double ans, num[MAX_N + 5];

double func(int i, int j) {
    double t1 = xy[i][0] - xy[j][0], t2 = xy[i][1] - xy[j][1];
    return sqrt(t1 * t1 + t2 * t2);
}

int main() {
    cin >> m;
    for (int i = 1; i <= m; ++i) {
        cin >> monkey[i];
    }
    cin >> n;
    for (int i = 1; i <= n; ++i) {
        cin >> xy[i][0] >> xy[i][1];
        num[i] = 1e9;
    }

    priority_queue<node> que;
    que.push((node){1, 0});
    num[1] = 0;
    while (!que.empty()) {
        node temp = que.top();
        que.pop();
        if (mark[temp.now] == 1) continue;
        mark[temp.now] = 1;
        ans = max(ans, temp.v);
        cnt++;
        if (cnt == n) break;
        for (int i = 1; i <= n; ++i) {
            if (mark[i] == 0) {
                double v = func(temp.now, i);
                if (v < num[i]) {
                    num[i] = v;
                    que.push((node) {i, v});
                }
            }
        }
    }

    int ansfin = 0;
    for (int i = 1; i <= m; ++i) {
        if (ans <= monkey[i]) ansfin++;
    }
    
    cout << ansfin << endl;

    return 0;
}
```

