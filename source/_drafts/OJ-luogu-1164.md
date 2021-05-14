---
title: OJ-luogu-1164
date: 2021-03-06 16:25:41
mathjax: true
tag: [OJ, luogu, 动态规划]
---

# [小A点菜](https://www.luogu.com.cn/problem/P1164)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210306165250004.png)

**输入**

```
4 4
1 1 2 2
```

**输出**

```
3
```

## 题解

令 $dp[i][j]$ 代表取前 $i$ 个物品，总代价为 $j$ 时，能获得的方案数
$$
\begin{equation}
	dp[i][j]=\left\{
		\begin{aligned}
		&dp[i-1][j],&买不起第 i 件\\ 
		&dp[i-1][j]+dp[i-1][j-a[i]], &买得起第 i 件
	\end{aligned}
	\right.
\end{equation}
$$



