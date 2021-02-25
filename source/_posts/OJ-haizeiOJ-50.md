---
title: OJ-haizeiOJ-50
date: 2021-02-25 10:38:23
mathjax: true
tags: [OJ, haizeiOJ, 动态规划, 递推]
---

# [扔鸡蛋](http://oj.haizeix.com/problem/50)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210225104322807.png)

**样例输入**

```
2 100
```

**样例输出**

```
14
```

**同类题目**

[leetcode](https://leetcode-cn.com/problems/super-egg-drop/)

---

## 题解 1

首先看懂题意，最坏情况最少测多少次可以测出鸡蛋的硬度，换句话说，就是求最保守的情况下，需要扔鸡蛋的最少次数。

在用第 $i$ 鸡蛋测第 $j$ 层楼时，如果鸡蛋碎了，那么需要使用剩下 $i - 1$ 个鸡蛋取测 $j - 1$ 层楼，如果没碎，那么这 $i$ 个鸡蛋可以继续去测楼的上半部分 $m - j$ 层。对于保守情况的理解，是说当鸡蛋出现碎和没碎的情况，是取其中较大值作为可能性。对于最少次数的理解，是说楼层 $j$ 在可能出现的遍历值中取最小值。 

令 $dp[i][j]$ 代表用 $i$ 个鸡蛋，测 $j$ 层楼的次数的话，可以发现以下关系：

$dp[i][j]=\min\limits_{1\le k \le j}{\{\max\left\{\begin{aligned}&dp[i-1][k-1]+1&鸡蛋碎了\\&dp[i][j-k]+1 &鸡蛋没碎\end{aligned}\right.} \}$

第一版代码：

```cpp
#include <bits/stdc++.h>
using namespace std;

#define MAX_N 32
#define MAX_M 1000000
int dp[MAX_N + 5][MAX_M + 5];

int main() {
    int m, n;
    cin >> n >> m;
    // 初始化值
    for (int i = 0; i <= m; ++i) dp[1][i] = i; // 1 个鸡蛋测 j 层需要测 j 次
    for (int i = 2; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {
            dp[i][j] = j; // i 个鸡蛋测 j 层默认最大测 j 次
            for (int k = 1; k <= j; k++) {
                dp[i][j] = min(dp[i][j], max(dp[i - 1][k - 1], dp[i][j - k]) + 1);
            }
        }
    }

    cout << dp[n][m] << endl;
    return 0;
}
```

## 题解 2

根据状态转移方程，碎和没碎的增长数量趋势是相反的，$dp[i][j]$ 在 $i, j$ 较大时具有较大值，所以可以判断随着 $k$ 的增加，$dp[i][j-k]$ 减少，$dp[i-1][k-1]$ 增加，最优的 $k$ 值一定在这两者交界处，这样可以优化掉循环 $\min$ 逻辑：

第二版代码：

```cpp
#include <bits/stdc++.h>
using namespace std;

#define MAX_N 32
#define MAX_M 1000000
int dp[MAX_N + 5][MAX_M + 5];

int main() {
    int m, n;
    cin >> n >> m;
    // 初始化
    for (int i = 0; i <= m; ++i) dp[1][i] = i;
    for (int i = 2; i <= n; ++i) {
        dp[i][1] = 1;
        dp[i][2] = 2;
        // i 个鸡蛋测 1、2 层的数量可以确定， j 从 3 层开始遍历
        int k = 2;
        for (int j = 3; j <= m; ++j) {
            while (k < j && dp[i - 1][k - 1] < dp[i][j - k]) ++k; // 此处 while 改成 if 也行， j 和 k 是同步增长的
            dp[i][j] = max(dp[i - 1][k - 1], dp[i][j - k]) + 1; // 改成 if 时 dp[i][j] 取的目标还是最优值 
        }
    }
    cout << dp[n][m] << endl;
    return 0;
}
```

第二版代码（leetcode版）：

```cpp
class Solution {
public:
    int dp[110][10010] = {0}; 
    int superEggDrop(int n, int m) {
        for (int i = 0; i <= m; i++) dp[1][i] = i;
        for (int i = 2; i <= n; i++) {
            dp[i][1] = 1;
            dp[i][2] = 2;
            int k = 2;
            for (int j = 3; j <= m; j++) {
                if (k < j && dp[i - 1][k - 1] < dp[i][j - k]) ++k;
                dp[i][j] = max(dp[i - 1][k - 1], dp[i][j - k]) + 1;
            }
        }
        return dp[n][m];
    }
};
```

## 题解 3

第二版代码已经可以通过 leetcode 的判题测试，但是 haizeiOJ 的测试要求更严格，测试楼层数可以达到 $2^{31}$ 数量级，空间上无法满足。

对状态定义进行优化，发现自变量 层数 和因变量 测试次数 呈现相关性，随着 层数 增加，测试次数 增加，所以可以对调两者关系。

保守情况下，$i$ 个鸡蛋扔 $j$ 次能测的楼层数，为 $i - 1$ 个鸡蛋扔 $j - 1$ 次测的楼层数（碎了），多 1 个鸡蛋多扔 1 次，根据已知条件扔这一次可多测扔 $i$ 个鸡蛋扔 $j - 1$ 次能得到的楼层数 + 1。 

令 $f[i][j]$ 代表 $i$ 个鸡蛋扔 $j$ 次，最多能测多少层楼，转为递推问题。

$f[i][j] = f[i - 1][j - 1] + f[i][j - 1] + 1$

第三版代码：

```cpp
#include <bits/stdc++.h>
using namespace std;

#define MAX_N 35
#define MAX_K 50000

long long f[2][MAX_K + 5];

int solve(int n, int m) {
    if (n == 1) return m;
    for (int i = 1; i <= MAX_K; i++) f[1][i] = i; // 1 个鸡蛋测 j 次最多能测 j 层
    for (int i = 2; i <= n; i++) {
        for (int k = 1; k <= MAX_K; k++) {
            // 滚动数组，减少使用空间
            f[i % 2][k] = f[(i - 1) % 2][k - 1] + f[(i - 1) % 2][k - 1] + 1;
        }
    }
    int k = 1;
    while (f[n][k] < m) k++; // 返回该楼层数所需要测的次数
    return k;
}

int main() {
    int m, n;
    cin >> n >> m;
    cout << solve(n, m) << endl;
    return 0;
}
```

