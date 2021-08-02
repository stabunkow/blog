---
title: OJ-leetcode-589
date: 2021-05-19 10:00:02
tag: [OJ, leetcode, 二叉树]
---



# [N 叉树的前序遍历](https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210519194419465.png)

```cpp
class Solution {
public:
    int getSize(Node *root) {
        if (root == NULL) return 0;
        int sum = 1;
        for (int i = 0; i < root->children.size(); ++i) {
            sum += getSize(root->children[i]);
        }
        return sum;
    }

    void __preorder(Node *root, vector<int> &ret) {
        if (root == NULL) return ;
        ret.push_back(root->val);
        for (int i = 0; i < root->children.size(); ++i) {
            __preorder(root->children[i], ret);
        }
        return ;
    }

    vector<int> preorder(Node* root) {
        int n = getSize(root);
        vector<int> ret;
        __preorder(root, ret);
        return ret;
    }
};
```

