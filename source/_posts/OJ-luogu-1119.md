---
title: OJ-luogu-1119
date: 2021-03-01 20:25:41
tag: [OJ, luogu, 最短路算法]
mathjax: true
---



# [灾后重建](https://www.luogu.com.cn/problem/P1119)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210301203250889.png)

**样例输入**

```
4 5
1 2 3 4
0 2 1
2 3 1
3 1 2
2 1 4
0 3 5
4
2 0 2
0 1 2
0 1 3
0 1 4
```

**样例输出**

```
-1
-1
5
4
```

## 题解

本题是求多源最短路径，用 Floyd 算法即可，但是做出了一些限制了最外层的循环。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 200
#define INF 0x3f3f3f3f

int n, m, arr[MAX_N + 5][MAX_N + 5], num[MAX_N + 5], now;

int main() {
    memset(arr, 0x3f, sizeof(arr));
    cin >> n >> m;
    for (int i = 1; i <= n; ++i) {
        cin >> num[i];
        arr[i][i] = 0;
    }
    for (int i = 0; i < m; ++i) {
        int a, b, c;
        cin >> a >> b >> c;
        a++, b++; // 下标对齐
        arr[a][b] = arr[b][a] = c;
    }
    int q;
    cin >> q;
    while (q--) {
        int x, y, t;
        cin >> x >> y >> t;
        x++, y++; // 下标对齐
        // 保证了村庄重建时间不下降
        // 被拆分的 floyd 算法，到第 t 天时，可以选择哪些点去中转
        while (num[now] <= t && now <= n) {
            for (int i = 1; i <= n; ++i) {
                for (int j = 1; j <= n; ++j) {
                    arr[i][j] = min(arr[i][j], arr[i][now] + arr[now][j]);
                }
            }
            now++;
        }
        if (arr[x][y] == INF || num[x] > t || num[y] > t) {
            cout << -1 << endl;
        } else {
            cout << arr[x][y] << endl;
        }
    }

    return 0;
}
```

