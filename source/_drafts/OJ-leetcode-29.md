---
title: OJ-leetcode-29
date: 2021-05-20 10:00:04
tag: [OJ, leetcode, 二分搜索]
---

# [两数相除](https://leetcode-cn.com/problems/divide-two-integers/)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210520210438419.png)

```cpp
class Solution {
public:
    long long mul(long long a, long long b) {
        long long temp = a, ans = 0;
        while (b) {
            if (b & 1) ans += temp;
            temp += temp;
            b >>= 1;
        }
        return ans;
    }

    int divide(long long a, long long b) {
        if (a == 0) return 0;
        if (b == 0) return INT_MAX;
        int flag = 0;
        if (a > 0 && b < 0) flag = 1;
        if (a < 0 && b > 0) flag = 1;
        if (a < 0) a = -a;
        if (b < 0) b = -b;
        long long l = 0, r = a, mid;
        // 二叉搜索 111000
        while (l < r) {
            mid = (l + r + 1) >> 1;
            if (mul(mid, b) <= a) l = mid;
            else r = mid - 1;
        }
        if (flag) l = -l;
        if (l < INT_MIN || l > INT_MAX) return INT_MAX;
        return l;
    }
};
```

