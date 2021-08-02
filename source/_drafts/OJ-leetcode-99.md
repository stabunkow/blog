---
title: OJ-leetcode-99
date: 2021-05-20 10:00:04
tag: [OJ, leetcode, 二叉树]
---

# [恢复二叉搜索树](https://leetcode-cn.com/problems/recover-binary-search-tree/submissions/)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210520192455515.png)

以中序遍历，能顺序遍历二叉搜索树，同时保存根节点的值信息进行比较。

```cpp
class Solution {
public:
    TreeNode *l, *r, *pre;
    void inorder(TreeNode *root) {
        if (root == NULL) return ;
        inorder(root->left);
        if (pre && root->val < pre->val) {
            r = root;
            if (l == NULL) l = pre;
        }
        pre = root;
        inorder(root->right);
        return ;
    }

    void recoverTree(TreeNode* root) {
        l = r = pre = NULL;
        inorder(root);
        int temp = l->val;
        l->val = r->val;
        r->val = temp;
        return ;
    }
};
```

