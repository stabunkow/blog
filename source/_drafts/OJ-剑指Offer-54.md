---
title: OJ-剑指Offer-54
date: 2021-05-19 10:00:03
tag: [OJ, 剑指Offer, leetcode, 二叉树, 递归与分治]
---



# [二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210519195209119.png)

```cpp
class Solution {
public:
    int count(TreeNode *root) {
        if (root == NULL) return 0;
        return count(root->left) + count(root->right) + 1;
    }

    int kthLargest(TreeNode* root, int k) {
        int r_size = count(root->right);
        if (r_size >= k) return kthLargest(root->right, k);
        if (r_size + 1 == k) return root->val;
        return kthLargest(root->left, k - r_size - 1);
    }
};
```

