---
title: OJ-haizeiOJ-637
date: 2021-03-04 19:53:41
mathjax: true
tag: [OJ, haizeiOJ, 拓扑排序]
---

# [排序](http://oj.haizeix.com/problem/637)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210304212400425.png)

**样例输入1**

```
4 6
A<B
A<C
B<C
C<D
B<D
A<B
```

**样例输出1**

```
Sorted sequence determined after 4 relations: ABCD.1
```

**样例输入2**

```
3 2
A<B
B<A
```

**样例输出2**

```
Inconsistency found after 2 relations.
```

**样例输入3**

```
26 1
A<Z
```

**样例输出3**

```
Sorted sequence cannot be determined.
```

## 题解

检查具有由优先级，先检查是否成环（经过一次拓扑排序，存在入度为 0 的节点），然后再检查拓扑序列是否不唯一确定（有多个拓扑排序开始的起点，或者拓扑排序过程中某个节点是否可能存在 2 种以上的进入路径）。加的过程中加的过程中可以运行拓扑排序，根据确定序列情况提前退出。若加入完所有边还不能确定拓扑排序则为不确定。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 26

struct edge {
    int e, next;
};

int n, m, head[MAX_N + 5], edg_cnt, in_degree[MAX_N + 5], ans_cnt;
edge edg[MAX_N * MAX_N + 5];
char ans[MAX_N + 5];

void add_edg(char a, char b) {
    int s = a - 'A' + 1, e = b - 'A' + 1;
    edg[edg_cnt].e = e;
    edg[edg_cnt].next = head[s];
    head[s] = edg_cnt++;
    in_degree[e]++;
}

int func() {
    ans_cnt = 0;
    // 开辟一个新的数组（每次加入边的时候都可以先跑一遍拓扑序列，提前退出
    int tin[MAX_N + 5] = {0};
    queue<int> que;
    for (int i = 1; i <= n; ++i) {
        tin[i] = in_degree[i];
        if (tin[i] == 0) {
            que.push(i);
        }
    }

    // f1 起点的数量
    // f2 每一轮的总入队标记
    // f3 成环标记
    int f1 = que.size(), f2 = 0, f3 = 0;
    while (!que.empty()) {
        // f4 该节点具有唯一路径
        int t = que.front(), f4 = 0;
        que.pop();
        ans[ans_cnt++] = t + 'A' - 1; // 保存序列等待输出
        for (int i = head[t]; i != -1; i = edg[i].next) {
            int e = edg[i].e;
            tin[e]--;
            if (tin[e] == 0) {
                que.push(e);
                f4++;
            }
        }
        // 不存在唯一路径
        if (f4 > 1) {
            f2 = 1;
        }
    }
    // 优先判断是否成环
    for (int i = 1; i <= n; i++) {
        if (tin[i] != 0) {
            f3 = 1;
            break;
        }
    }
    
    if (f3 == 1) {
        return 2;
    }
    // 起点数量不唯一或节点可能由多个拓扑序列路径经过，那么拓扑序列不唯一
    if (f1 > 1 || f2 == 1) {
        return 0;
    }
    return 1;
}

int main() {
    memset(head, -1, sizeof(head));
    cin >> n >> m;
    for (int i = 1; i <= m; ++i) {
        char a, b;
        cin >> a >> b >> b;
        add_edg(a, b);
        // 加入边的过程可以提前运行拓扑排序
        int t = func();
        if (t == 1) {
            printf("Sorted sequence determined after %d relations: %s.\n", i, ans);
            return 0;
        } else if (t == 2) {
            printf("Inconsistency found after %d relations.\n", i);
            return 0;
        }
    }
    printf("Sorted sequence cannot be determined.\n");
    return 0;
}
```

