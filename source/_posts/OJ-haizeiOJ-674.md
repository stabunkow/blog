---
title: OJ-haizeiOJ-675
date: 2021-03-06 19:25:41
mathjax: true
tag: [OJ, haizeiOJ, 最短路]
---

# [宿舍楼里的电竞赛](http://oj.haizeix.com/problem/674)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210308172514994.png)

**样例输入**

```
4 3
1 2
1 3
4 1
```

**样例输出**

```
2
```

## 题解

1 个队伍的情况确定需要跟其他 n- 1个队伍有关系，利用多源最短路算法 Floyd，求每支队伍跟其他队伍联系的情况，然后利用出度值与入度值之和为 n - 1 认为这只队伍有确定的排名情况。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 100
#define INF 0x3f3f3f3f

int n, m, arr[MAX_N + 5][MAX_N + 5], in_degree[MAX_N + 5], out_degree[MAX_N], ans;

int main() {
    memset(arr, 0x3f, sizeof(arr));
    cin >> n >> m;
    
    for (int i = 0; i < m; ++i) {
        int a, b;
        cin >> a >> b;
        arr[a][b] = 1;
    }

    for (int k = 1; k <= n; ++k) {
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= n; ++j) {
                arr[i][j] = min(arr[i][j], arr[i][k] + arr[k][j]);
            }
        }
    }
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= n; ++j) {
            if (arr[i][j] != INF) {
                out_degree[i]++;
                in_degree[j]++;
            }
        }
    }

    for (int i = 1; i <= n; ++i) {
        if (in_degree[i] + out_degree[i] == n - 1) {
            ans++;
            // i 的实际排名为 in_degree[i] + 1
        }
    }

    cout << ans << endl;

    return 0;
}
```



