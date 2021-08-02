---
title: OJ-leetcode-106
date: 2021-05-19 10:00:04
tag: [OJ, leetcode, 二叉树, 递归与分治]
---

# [从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210520154502593.png)

```cpp
class Solution {
public:
    TreeNode *getNewNode(int key) {
        TreeNode *p = new TreeNode;
        p->val = key;
        p->left = p->right = NULL;
        return p;
    }

    TreeNode* __buildTree(int *inorder, int *postorder, int size) {
        if (size == 0) return NULL;
        int key = postorder[size - 1];
        TreeNode *root = getNewNode(key);
        int ind = 0, n = size;
        while (inorder[ind] != key) ++ind;
        root->left = __buildTree(inorder, postorder, ind);
        root->right = __buildTree(inorder + ind + 1, postorder + ind, n - ind - 1);
        return root;
    }

    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        return __buildTree(inorder.data(), postorder.data(), inorder.size());
    }
};
```

