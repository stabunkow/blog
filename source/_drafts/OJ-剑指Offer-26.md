---
title: OJ-剑指Offer-26
date: 2021-05-19 10:00:03
tag: [OJ, 剑指Offer, leetcode, 二叉树, 递归与分治]
---

# [树的子结构](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210519211104093.png)

```cpp
class Solution {
public:
    bool isMatch(TreeNode *A, TreeNode *B) {
        if (B == NULL) return true;
        if (A == NULL) return false;
        if (A->val != B->val) return false;
        return isMatch(A->left, B->left) && isMatch(A->right, B->right);
    }

    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if (B == NULL || A == NULL) return false;
        if (A->val == B->val && isMatch(A, B)) return true;
        return isSubStructure(A->left, B) || isSubStructure(A->right, B); 
    }
};
```

