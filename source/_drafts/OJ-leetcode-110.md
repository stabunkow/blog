---
title: OJ-leetcode-110
date: 2021-05-19 10:00:01
tag: [OJ, leetcode, 二叉树]
---



# [平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210519192615326.png)

通过递归向上传递子树的不平衡数值信息。

```cpp
class Solution {
public:
    int getHeight(TreeNode *root) {
        if (root == NULL) return 0;
        int l = getHeight(root->left);
        int r = getHeight(root->right);
        // 已经不平衡，其后判别式肯定也不平衡
        if (l == -2 || r == -2) return -2;
        if (abs(l - r) > 1) return -2;
        return max(l, r) + 1;
    }

    bool isBalanced(TreeNode* root) {
        return getHeight(root) >= 0;
    }
};
```

