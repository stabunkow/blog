---
title: OJ-haizeiOJ-242
date: 2021-05-21 10:00:04
tag: [OJ, haizeiOJ, 二分搜索]
---

# [最大平均值](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/submissions/)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210521103420705.png)

**样例输入**

```
10 6
6 
4
2
10
3
8
5
9
4
1
```

**样例输出**

```
6500
```

求连续串可用的平均值时，如果串值的和大于 平均值 * 串长，说明平均值还可以增加。

以该函数来找可用的平均值，可用的平均值会单调上升，可用二分搜索来查找。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 100000

long long n, m, l = 0, r = 0;
long long arr[MAX_N + 5];

int f(int x, int m) {
    arr[0] = x;
    long long pre = 0, si = 0, sj = 0;
    for (int i = 1; i <= n; ++i) {
        si += (arr[i] - x);
        if (i < m) continue;
        sj += (arr[i - m] - x); 
        pre = min(pre, sj);
        if (si >= pre) return 1; // 找到平均值，长度为 m 以上时正好抵消为 0
    }
    return 0;
}

int bs(int l, int r, int m) {
    if (l == r) return l;
    int mid = (l + r + 1) >> 1;
    if (f(mid, m)) l = mid;
    else r = mid - 1;
    return bs(l, r, m);
}

int main() {
    cin >> n >> m;
    // 查找平均值
    for (int i = 1; i <= n; ++i) {
        cin >> arr[i];
        arr[i] *= 1000;
        if (i == 1) l = r = arr[i];
        l = min(l, arr[i]);
        r = max(r, arr[i]);
    }
    cout << bs(l, r, m) << endl;
    return 0;
}
```

