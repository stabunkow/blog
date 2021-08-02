---
title: OJ-leetcode-29
date: 2021-05-20 10:00:04
tag: [OJ, leetcode, 递归与分治]
---

# [组合总和](https://leetcode-cn.com/problems/combination-sum/)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210520203249190.png)

```cpp
class Solution {
public:
    vector<vector<int>> ret;
    vector<int> nums;
    
    void getResult(int k, int target, vector<int> now) {
        if (k == nums.size() || target < 0) return ;
        if (target == 0) {
            ret.push_back(now);
            return ;
        }
        getResult(k + 1, target, now);
        now.push_back(nums[k]);
        getResult(k, target - nums[k], now);
        return ;
    }

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        nums = candidates;
        getResult(0, target, vector<int>());
        return ret;
    }
};
```

