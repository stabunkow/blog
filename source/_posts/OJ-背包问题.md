---
title: OJ-背包问题
date: 2021-03-06 15:06:41
mathjax: true
tag: [OJ, haizeiOJ, 动态规划]
---

# 背包问题

## [0/1背包](http://oj.haizeix.com/problem/47)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210306150849316.png)

**样例输入**

```
15 4
4 10
3 7
12 12
9 8
```

**样例输出**

```
19
```

### 题解1

令 $dp[i][j]$ 代表取前 $i$ 个物品，背包剩余承重为 $j$ 的情况下的价值
$$
\begin{equation}
	dp[i][j]=\max\left\{
		\begin{aligned}
		&dp[i-1][j],&没选第 i 件\\ 
		&dp[i-1][j-w[i]]+v[i], &选了第 i 件
	\end{aligned}
	\right.
\end{equation}
$$
用图表示，往下和往右，代表背包价值和对 $i$ 件物品的选取情况。

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210127105205497.png)

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 100
#define MAX_W 10000

int v[MAX_N + 5], w[MAX_N + 5];
int dp[MAX_N][MAX_W + 5];

int main() {
    int W, n;
    cin >> W >> n;
    for (int i = 1; i <= n; ++i) {
        cin >> w[i] >> v[i];
    }
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= W; ++j) {
            dp[i][j] = dp[i - 1][j]; // 放不下默认不选
            if (j >= w[i]) { // 如果能够剩余容量放的下
                // 选和不选两种可能的最大值
                dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - w[i]] + v[i]);
            }
        }
    }

    cout << dp[n][W] << endl;

    return 0;
}
```

### 题解2

观察 0/1 背包的图表示情况，从左到右 $dp[i][j]$ 依赖于上一层左侧和正上方的值，下一轮后不需要上上层的信息，可以改成滚动数组减少空间使用。根据图，更新值需要根据上层的值和左上方的值（左上方的值需要保留上层的值，从左往右值会被更新污染，从右到左遍历在更新之前依然保留上的值。

```cpp
#include <bits/stdc++.h>
using namespace std;

#define MAX_W 10000

int dp[MAX_W + 5];

int main() {
    int W, n, v, w;
    cin >> W >> n;
    for (int i = 1; i <= n; ++i) {
        scanf("%d%d", &w, &v);
        // 逆方向遍历，下标较小的 dp 数组元素还是上一层的值
        for (int j = W; j >= w; --j) {
            dp[j] = max(dp[j], dp[j - w] + v);
        }
    }

    printf("%d\n", dp[W]);

    return 0;
}
```

## [完全背包](http://oj.haizeix.com/problem/48)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210306152840829.png)

**样例输入**

```
5 20
2 3
3 4
10 9
5 2
11 11
```

**样例输出**

```
30
```

### 题解1

令 $dp[i][j]$ 代表取前 $i$ 个物品，背包剩余承重为 $j$ 的情况下的价值
$$
\begin{equation}
	dp[i][j]=\max\left\{
		\begin{aligned}
		&dp[i-1][j],&没选第 i 件\\ 
		&dp[i][j-w[i]]+v[i], &选了若干第 i 件
	\end{aligned}
	\right.
\end{equation}
$$

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 100
#define MAX_W 10000

int v[MAX_N + 5], w[MAX_N + 5];
int dp[MAX_N][MAX_W + 5];

int main() {
    int W, n;
    cin >> n >> W;
    for (int i = 1; i <= n; ++i) {
        cin >> w[i] >> v[i];
    }
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= W; ++j) {
            dp[i][j] = dp[i - 1][j]; // 放不下默认不选
            if (j >= w[i]) { // 如果能够剩余容量放的下
                // 在选取后能否再次选取物品
                dp[i][j] = max(dp[i - 1][j], dp[i][j - w[i]] + v[i]);
            }
        }
    }

    cout << dp[n][W] << endl;

    return 0;
}
```

### 题解2

改成滚动数组，从左到右 $dp[i][j]$ 依赖于上一层左侧和正上方的值。从前往后遍历 $dp[i][j]$ 价值的数据需要不断更新，依赖更新值。

```cpp
#include <bits/stdc++.h>
using namespace std;

#define MAX_W 10000

int dp[MAX_W + 5];

int main() {
    int W, n, v, w;
    cin >> n >> W;

    for (int i = 1; i <= n; ++i) {
        scanf("%d%d", &w, &v);
        // 正方向遍历，dp[j] 会被更新
        for (int j = w; j <= W; ++j) {
            dp[j] = max(dp[j], dp[j - w] + v);
        }
    }

    printf("%d\n", dp[W]);

    return 0;
}
```

## [多重背包](http://oj.haizeix.com/problem/49)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210306154336857.png)

**输入样例**

```
15 4
4 10 5
3 7 4
12 12 2
9 8 7
```

**输出样例**

```cpp
37
```

### 题解

把多重背包的每类物品数量限制，当作多个单一物品来处理，转成复合 0/1 背包问题，直接使用滚动数组完成里层循环。

状态转移过程上进行优化，用 **二进制** 拆分每类物品，拆分后迭代次数会更少。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_W

int dp[MAX_W + 5];

int main() {
    int W, n, w, v, s;
    cin >> W >> n;

    for (int i = 1; i <= n; ++i) {
        scanf("%d%d%d", &w, &v, &s);
        for (int k = 1; s; k *= 2) {
            if (k > s) k = s;
            s -= k;
            for (int j = W; j >= k * w; --j) {
                dp[j] = max(dp[j], dp[j - k * w] + k * v);
            }
        }
    }

    printf("%d\n", dp[W]);

    return 0;
}
```

