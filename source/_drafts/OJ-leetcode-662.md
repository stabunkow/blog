---
title: OJ-leetcode-662
date: 2021-05-19 10:00:04
tag: [OJ, leetcode, 二叉树]
---

# [二叉树最大宽度](https://leetcode-cn.com/problems/maximum-width-of-binary-tree/)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210519214955935.png)

层序遍历，并使用相关树节点的索引信息。

```cpp
class Solution {
public:
    struct Data_T {
        int ind;
        TreeNode *node;
    };
    
    int getSize(TreeNode *root) {
        if (root == NULL) return 0;
        return getSize(root->left) + getSize(root->right) + 1;
    }

    int widthOfBinaryTree(TreeNode* root) {
        int n = getSize(root), ans = 0;
        queue<Data_T> que;
        que.push((Data_T) {0, root});
        while (!que.empty()) {
            int cnt = que.size();
            int l = que.front().ind, r;
            for (int i = 0; i < cnt; ++i) {
                Data_T d = que.front();
                int ind = d.ind - l;
                que.pop();
                if (d.node->left) que.push((Data_T) {2 * ind, d.node->left});
                if (d.node->right) que.push((Data_T) {2 * ind + 1, d.node->right});
                r = d.ind;
            }
            if (r - l + 1 > ans) ans = r - l + 1;
        }
        return ans;
    }
};
```

