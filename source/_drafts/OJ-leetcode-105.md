---
title: OJ-leetcode-105
date: 2021-05-19 10:00:04
tag: [OJ, leetcode, 二叉树, 递归与分治]
---

# [从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210519195841293.png)

```cpp
class Solution {
public:
    TreeNode* getNewNode(int key) {
        TreeNode *p = new TreeNode;
        p->val = key;
        p->left = p->right = NULL;
        return p;
    }

    TreeNode* __buildTree(int *preorder, int *inorder, int size) {
        if (size == 0) return NULL;
        int key = preorder[0], n = size;
        TreeNode *root = getNewNode(key);
        int ind = 0;
        while (inorder[ind] != key) ++ind;
        root->left = __buildTree(preorder + 1, inorder, ind);
        root->right = __buildTree(preorder + ind + 1, inorder + ind + 1, n - ind - 1);
        return root;
    }

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        return __buildTree(preorder.data(), inorder.data(), preorder.size());
    }
};
```

