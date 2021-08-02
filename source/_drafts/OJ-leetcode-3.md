---
title: OJ-leetcode-3
date: 2021-05-20 10:00:04
tag: [OJ, leetcode, 二分搜索]
---

# [无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/submissions/)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210520210248441.png)

利用计数关系，确保检查到长度为 l 的连续的串。

```cpp
class Solution {
public:
    int check(string s, int l) {
        int cnt[128] = {0};
        int k = 0;
        for (int i = 0; i < s.length(); ++i) {
            cnt[s[i]] += 1;
            if (cnt[s[i]] == 1) k += 1;
            if (i >= l) {
                cnt[s[i - l]] -= 1;
                if (cnt[s[i - l]] == 0) k -= 1;
            }
            if (k == l) return 1;
        }
        return 0;
    }
    
    int lengthOfLongestSubstring(string s) {
        if (s == "") return 0;
        int mid, l = 1, r = s.length();
        while (l < r) {
            mid = (l + r + 1) >> 1;
            if (check(s, mid)) l = mid;
            else r = mid - 1;
        }
        return l;
    }
};
```

