---
title: OJ-leetcode-107
date: 2021-05-19 10:00:04
tag: [OJ, leetcode, 二叉树]
---

# [二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210519213544013.png)

```cpp
class Solution {
public:
    int getHeight(TreeNode *root) {
        if (root == NULL) return 0;
        int l = getHeight(root->left);
        int r = getHeight(root->right);
        return max(l, r) + 1;
    }

    void getColSize(TreeNode *root, int k, vector<int> &colSize) {
        if (root == NULL) return ;
        colSize[k]++;
        getColSize(root->left, k + 1, colSize);
        getColSize(root->right, k + 1, colSize);
        return ;
    }

    void getResult(TreeNode *root, int k, vector<int> &colSize, vector<vector<int>> &ret) {
        if (root == NULL) return ;
        ret[k][colSize[k]++] = root->val;
        getResult(root->left, k + 1, colSize, ret);
        getResult(root->right, k + 1, colSize, ret);
        return ;
    }

    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        int n = getHeight(root);
        vector<vector<int>> ret;
        vector<int> colSize;
        ret.resize(n);
        colSize.resize(n, 0);
        getColSize(root, 0, colSize);
        for (int i = 0; i < n; ++i) {
            ret[i].resize(colSize[i]);
            colSize[i] = 0;
        }
        getResult(root, 0, colSize, ret);
        reverse(ret.begin(), ret.end());
        return ret;
    }
};
```

