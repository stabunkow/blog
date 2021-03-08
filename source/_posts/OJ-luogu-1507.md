---
title: OJ-luogu-1507
date: 2021-03-06 16:25:41
mathjax: true
tag: [OJ, luogu, 动态规划]
---

# [NASA的食物计划](https://www.luogu.com.cn/problem/P1507)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210306163127141.png)

**输入**

```
320 350
4
160 40 120
80 110 240
220 70 310
40 400 220
```

**输出**

```
550
```

## 题解1

令 $dp[i][j][k]$ 代表取前 $i$ 个物品，背包剩余体积为 $j$，剩余承重位 $k$ 的情况下的价值
$$
\begin{equation}
	dp[i][j][k]=\max\left\{
		\begin{aligned}
		&dp[i-1][j][k],&没选第 i 件\\ 
		&dp[i-1][j-vol[i]][k-wei[i]]+vol[i], &选了第 i 件
	\end{aligned}
	\right.
\end{equation}
$$


```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 50
#define MAX_M 400

int n, V, W, vol[MAX_N + 5], wei[MAX_N + 5], val[MAX_N + 5];
int dp[MAX_N + 5][MAX_M + 5][MAX_M + 5];

int main() {
    cin >> V >> W >> n;

    for (int i = 1; i <= n; ++i) {
        cin >> vol[i] >> wei[i] >> val[i]; 
    }

    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= V; ++j) {
            for (int k = 1; k <= W; ++k) {
                if (j <= vol[i] || k <= wei[i]) {
                    dp[i][j][k] = dp[i - 1][j][k];
                } else {
                    dp[i][j][k] = max(dp[i - 1][j][k], dp[i - 1][j - vol[i]][k - wei[i]] + val[i]);
                }
            }
        }
    }

    cout << dp[n][V][W] << endl;

    return 0;
}
```

## 题解2

因为体积和质量是同一时间变化的，可以同 0/1 背包一样进行空间优化。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_M 400

int n, V, W;
int dp[MAX_M + 5][MAX_M + 5];

int main() {
    cin >> V >> W >> n;
    int vol, wei, val;
    for (int i = 1; i <= n; ++i) {
        cin >> vol >> wei >> val;
        for (int j = V; j >= vol; --j) {
            for (int k = W; k >= wei; --k) {
                dp[j][k] = max(dp[j][k], dp[j - vol][k - wei] + val);
            }
        }
    }

    cout << dp[V][W] << endl;

    return 0;
}
```

