---
title: OJ-二叉树的遍历
date: 2021-03-01 13:25:41
tag: [OJ, 二叉树, 分治与递归]
---

# 二叉树的遍历

通过前序遍历和中序遍历、或中序遍历和后序遍历可获知一棵二叉树的结构，前序第一个是根节点，中序根节点左右两部分是其子树，后序最后一个是根节点。

通过前序遍历和中序遍历获得后序遍历，以递归方式易实现。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 100

char q[MAX_N + 5], z[MAX_N + 5];

void func(int ql, int qr, int zl, int zr) {
    if (ql > qr) {
        return ;
    }
    // 只含一个字符
    if (ql == qr) {
        cout << q[ql];
        return ;
    }
    // 前序第一个节点一定是根节点
    char root = q[ql];
    // 找到根的分割位置
    int ind = zl;
    while (z[ind] != root) {
        ind++;
    }
    // 前序中左右 中序左中右
    // 遍历根左半部分
    // ql + 1, ql + 1 + ind - zl - 1
    func(ql + 1, ql + ind - zl, zl, ind - 1);
    // 遍历根右半部分
    func(ql + ind - zl + 1, qr, ind + 1, zr);
    cout << root;
}

int main() {
    cin >> q >> z;
    func(0, strlen(q) - 1, 0, strlen(z) - 1);
    cout << endl;
    return 0;
}
```

**输入：**

```
ABVZCG
VBZCAG
```

**输出：**

```
VCZBGA
```

通过中序遍历，后序遍历获得前序遍历：

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 100

char z[MAX_N + 5], h[MAX_N + 5];

void func(int zl, int zr, int hl, int hr) {
    if (zl > zr) {
        return ;
    }
    if (zl == zr) {
        cout << z[zl];
        return ;
    }
    char root = h[hr];
    int ind = zl;
    while (z[ind] != root) {
        ind++;
    }
    cout << root;
    // 遍历根左半部分
    func(zl, ind - 1, hl, hl + ind - zl - 1);
    // 遍历根右半部分
    func(ind + 1, zr, ind + ind - zl, hr - 1);
}

int main() {
    cin >> z >> h;
    func(0, strlen(z) - 1, 0, strlen(h) - 1);
    cout << endl;
    return 0;
}
```

**输入：**

```
VBZCAG
VCZBGA
```

**输出：**

```
ABVZCG
```



