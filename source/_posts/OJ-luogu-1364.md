---
title: OJ-luogu-1364
date: 2021-02-26 20:25:41
tag: [OJ, luogu, 最短路算法]
mathjax: true
---



# [医院设置](https://www.luogu.com.cn/problem/P1364)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210226211556530.png)

**样例输入**

```
5						
13 2 3
4 0 0
12 4 5
20 0 0
40 0 0
```

**样例输出**

```
81
```

## 题解

本题可以把树结构理解为图，用 Floyd 算法，求多源最短路径之后，再根据题意，计算最小距离和。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 100

int n, arr[MAX_N + 5][MAX_N + 5], num[MAX_N + 5];

int main() {
    memset(arr, 0x3f, sizeof(arr));
    cin >> n;
    for (int i = 1; i <= n; ++i) {
        int a, b;
        cin >> num[i] >> a >> b;
        if (a != 0) {
            arr[i][a] = arr[a][i] = 1;
        }
        if (b != 0) {
            arr[i][b] = arr[b][i] = 1;
        }
        arr[i][i] = 0;
    }

    for (int k = 1; k <= n; ++k) {
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= n; ++j) {
                arr[i][j] = min(arr[i][j], arr[i][k] + arr[k][j]);
            }
        }
    }

    int ans = 1e9;
    for (int i = 1; i <= n; ++i) {
        int t = 0;
        for (int j = 1; j <= n; ++j) {
            t += num[j] * arr[j][i];
        }
        ans = min(ans, t);
    }

    cout << ans << endl;

    return 0;
}
```

