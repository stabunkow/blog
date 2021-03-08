---
title: OJ-morris遍历
date: 2021-03-01 14:25:41
tag: [OJ, leetcode, 二叉树]
mathjax: true
---

# morris 遍历

给定一个二叉树的根节点，返回它的前、中、后序遍历结果。

一般来说可以用递归简单实现，或是用栈来遍历模拟递归栈空间。

morris 遍历可以将非递归遍历中的空间复杂度降为 $O(1)$，利用树的空闲指针，建立了一种访问机制：对于**没有左子树的节点只到达一次，对于有左子树的节点会到达两次**，利用空闲指针来建立从前驱（中序的前驱）返回访问。该过程比较抽象，可以用笔纸来模拟一遍。

## 中序遍历

题目：[二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        while (root != nullptr) {
			// 无前驱
            if (root->left == nullptr) {
                // 输出根节点后遍历右侧
                ans.push_back(root->val);
                root = root->right; 
            } else {
                TreeNode *pre = root->left;
                // 找前驱节点
                while (pre->right != nullptr && pre->right != root) {
                    pre = pre->right;
                }
                // 第一次访问，使得前驱可以返回
                if (pre->right == nullptr) {
                    pre->right = root;
                    root = root->left;
                } else {
                    // 第二次访问
                    ans.push_back(root->val);
                    pre->right = nullptr;
                    // 从前驱返回，删除对原树的改动
                    root = root->right;
                }
            }
        }
        return ans;
    }
};
```

## 前序遍历

题目：[二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> ans;
        while (root != nullptr) {
            TreeNode *pre = root->left;
            while (pre && pre->right != nullptr && pre->right != root) {
                pre = pre->right;
            }
            // 如果前驱不存在
            if (pre == nullptr) {
                ans.push_back(root->val);
                root = root->right;
            } else {
                 // 第一次访问，使得前驱可以返回
                if (pre->right == nullptr) {
                    pre->right = root;
                    ans.push_back(root->val);
                    root = root->left;
                } else {
                    // 第二次访问，删掉对原树的修改
                    // 此处会先经过前驱不存在, root = root->right
                    pre->right = nullptr;
                    root = root->right;
                }
            }
        }
        return ans;
    }
};
```

## 后序遍历

题目：[二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)

后序遍历相对麻烦一些，需要更多步骤。根据从前驱返回的规则，我们可以发现从前驱返回，要访问的根节点顺着右边访问的顺序需要逆序一下才是后序遍历，另外需要建立一个虚拟根节点使其能够访问原根节点顺着右边访问的那一排。

```cpp
class Solution {
public:
    void add(vector<int> &ans, TreeNode *p) {
        int cnt = 0;
        // 顺着右边访问，逆序返回才是后序对这排的后序遍历
        while (p != nullptr) {
            ans.push_back(p->val);
            p = p->right;
            cnt++;
        }
        reverse(ans.end() - cnt, ans.end());
    }
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> ans;
        TreeNode *p = new TreeNode(-1); // 虚拟根节点
        p->left = root;
        while (p != nullptr) {
            // 没有前驱
            if (p->left == nullptr) {
                p = p->right;
            } else {
                TreeNode *pre = p->left;
                // 找前驱节点
                while (pre->right != nullptr && pre->right != p) {
                    pre = pre->right;
                }
                // 第一次访问，使得前驱可以返回
                if (pre->right == nullptr) {
                    pre->right = p;
                    p = p->left;
                } else {
                    // 第二次访问，删除对原树的改动，并使其后作为前驱能够后序编出来
                    pre->right = nullptr;
                    add(ans, p->left); // 把前驱那排后序遍历出来
                    p = p->right;
                }
            }
        }
        return ans;
    }
};
```

