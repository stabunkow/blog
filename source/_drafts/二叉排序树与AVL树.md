---
title: 二叉排序树与AVL树
date: 2021-05-19 10:00:00
tag: [OJ, haizeiOJ, 二叉树]
---

# 二叉排序树

左子树 < 根节点 < 右子树

只根据前序、或后序遍历结果就能还原这棵二叉排序树。

```cpp
#include <bits/stdc++.h>

using namespace std;

struct Node {
    int key, size;
    Node *lchild, *rchild;
};

Node __NIL;
#define NIL (&__NIL)

__attribute__((constructor))
void init_NIL() {
    NIL->key = 0;
    NIL->size = 0;
    NIL->lchild = NIL->rchild = NIL;
    return ;
}

Node *getNewNode(int key) {
    Node *p = (Node *) malloc(sizeof(Node));
    p->key = key;
    p->size = 1;
    p->lchild = p->rchild = NIL;
    return p;
}

void update_size(Node *root) {
    root->size = root->lchild->size + root->rchild->size;
    return ;
}

Node *insert(Node *root, int key) {
    if (root == NIL) return getNewNode(key);
    if (root->key == key) return root;
    if (root->key > key) root->lchild = insert(root->lchild, key);
    else root->rchild = insert(root->rchild, key);
    update_size(root);
    return root;
}

Node *predecessor(Node *root) {
    Node *temp = root->lchild;
    while (temp->rchild != NIL) temp = temp->rchild;
    return temp;
}

Node *erase(Node *root, int key) {
    if (root == NIL) return root;
    if (root->key > key) {
        root->lchild = erase(root->lchild, key);
    } else if (root->key < key) {
        root->rchild = erase(root->rchild, key);
    } else {
        if (root->lchild == NIL || root->rchild == NIL) {
            Node *temp = root->lchild ? root->lchild : root->rchild;
            free(root);
            return temp;
        } else {
            Node *temp = predecessor(root);
            root->key = temp->key;
            root->lchild = erase(root->lchild, temp->key);
        }
    }
    update_size(root);
    return ;
}

void clear(Node *root) {
    if (root == NIL) return ;
    clear(root->lchild);
    clear(root->rchild);
    free(root);
    return ;
}

Node *rand_insert(Node *root) {
    root = insert(root, rand() % 30);
    return root;
}

void pre_order(Node *root) {
    if (root == NIL) return ;
    printf("%d ", root->key);
    pre_order(root->lchild);
    pre_order(root->rchild);
    return ;
}

void in_order(Node *root) {
    if (root == NIL) return ;
    in_order(root->rchild);
    printf("%d ", root->key);
    in_order(root->lchild);
    return ;
}

void post_order(Node *root) {
    if (root == NIL) return ;
    post_order(root->rchild);
    post_order(root->lchild);
    printf("%d ", root->key);
    return ;
}

void find_k(Node *root, int k) {
    if (root->rchild->size >= k) return find_k(root->rchild, k);
    if (root->rchild->size + 1 == k) return root->key;
    return find_k(root->lchild, k - root->rchild->size - 1);
}

int main() {
    srand(time(0));
    int n;
    scanf("%d", &n);
    Node *root = NIL;
    for (int i = 0; i < n; ++i) {
        root = rand_insert(root);
    }
    pre_order(root); printf("\n");
    in_order(root); printf("\n");
    post_order(root); printf("\n");
    int k;
    while (~scanf("%d", &k)) {
        printf("find %dth element = %d\n", k, find_k(root, k));
    }
    return 0;
}

```

# AVL 树

平衡二叉排序树，具有二叉排序树的性质的同时，左右子树高度差不超过 1。

另外还有 Size Balanced 树，要求每棵子树的大小不小于其兄弟的子树大小。

```cpp
#include <bits/stdc++>

using namespace std;

struct Node {
  	int key, h;
    Node *lchild, *rchild;
};

Node __NIL;
#define NIL (&__NIL)

__attribute__((constructor))
void init_NIL() {
    NIL->key = -1;
    NIL->lchild = NIL->rchild = NIL;
    NIL->h = 0;
    return ;
}

void update_height(Node *root) {
    root->h = max(root->lchild->h, root->rchild->h) + 1;
    return ;
}

Node *getNewNode(int key) {
    Node *p = (Node *) malloc(sizeof(Node));
    p->key = key;
    p->lchild = p->rchild = NIL;
    p->h = 1;
    return p;
}

Node *left_rotate(Node *root) {
    Node *temp = root->rchild;
    temp->lchild = root;
    root->rchild = temp->lchild;
    update_height(root);
    update_height(temp);
    return temp;
}

Node *right_rotate(Node *root) {
    Node *temp = root->lchild;
    temp->rchild = root;
    root->lchild = temp->rchild;
    update_height(root);
    update_height(temp);
    return temp;
}

Node *maintain(Node *root) {
    if (abs(root->lchild->h - root->rchild->h) <= 1) return root;
    if (root->lchild->h > root->rchild->h) {
        if (root->lchild->rchild->h > root->lchild->lchild->h) {
            root->lchild = left_rotate(root->lchild);
        }
        root = right_rotate(root);
    } else {
        if (root->rchild->lchild->h > root->rchild->rchild->h) {
            root->rchild = right_rotate(root->rchild);
        }
        root = left_rotate(root);
    }
    return root;
}

Node *insert(Node *root, int key) {
    if (root == NIL) return getNewNode(key);
    if (root->key == key) return root;
    if (key < root->key) {
        root->lchild = insert(root->lchild, key);
    } else {
        root->rchild = insert(root->rchild, key);
    }
    update_height(root);
    return maintain(root);
}

Node *predecessor(Node *root) {
    Node *temp = root->lchild;
    while (temp->rchild != NIL) temp = temp->rchild;
    return temp;
}

Node *erase(Node *root, int key) {
    if (root == NIL) return root;
    if (key < root->key) {
        root->lchild = erase(root->lchild, key);
    } else if (key > root->key) {
        root->rchild = erase(root->rchild, key);
    } else {
        if (root->lchild == NIL || root->rchild == NIL) {
            Node *temp = (root->lchild == NIL ? root->rchild : root->lchild);
            free(root);
            return temp;
        } else {
            Node *temp = predecssor(root);
            root->key = temp->key;
            root->lchild = erase(root->lchild, temp->key);
        }
    }
    update_height(root);
    return maintain(root);
}

void clear(Node *root) {
    if (root == NIL) return ;
    clear(root->lchild);
    clear(root->rchild);
    free(root);
    return ;
}

void output(Node *root) {
    if (root == NIL) return ;
    printf("(%d(%d) | %d, %d)\n", root->key, root->h, root->lchild->key, root->rchild->key);
    output(root->lchild);
    output(root->rchild);
    return ;
} 

Node *rand_insert(Node *root) {
    root = insert(root, rand() % 100);
    output(root);
    return root;
}

int main() {
    srand(time(0));
    int n;
    scanf("%d", &n);
    Node *root = NIL;
    for (int i = 0; i < n; ++i) {
        root = rand_insert(root);
    }
    clear(root);
    return ;
}
```

