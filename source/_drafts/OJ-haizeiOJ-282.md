---
title: OJ-haizeiOJ-282
date: 2021-05-21 10:00:04
tag: [OJ, haizeiOJ, 字典树]
---

# [最大异或对](http://oj.haizeix.com/problem/282)

![](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210522160334011.png)

**样例输入**

```
3
1 2 3
```

**样例输出**

```
3
```

通过形成二叉字典树，可以使得一个串与其他串的异或比较，可以同时进行。

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_N 100000
#define BASE 31
struct node {
    node *next[2];
} tree[MAX_N * BASE + 5];
int cnt = 0;
node *getNode() {
    return &tree[cnt++];
}

void insert(node *root, int x) {
    for (int i = 30; i >= 0; i--) {
        int ind = !!(x & (1 << i)); // 01 转换
        if (root->next[ind] == NULL) root->next[ind] = getNode();
        root = root->next[ind];
    }
    return ;
}

int query(node *root, int x) {
    int ans = 0;
    for (int i = 30; i >= 0; i--) {
        int ind = !(x & (1 << i)); // 异或获取
        if (root->next[ind]) {
            ans |= (1 << i);
            root = root->next[ind];
        } else {
            root = root->next[!ind];
        }
    }
    return ans;
}

int n;
int val[MAX_N + 5];
int main() {
	cin >> n;
    int ans = 0, a;
    node *root = getNode();
    cin >> a, n--;
    insert(root, a);
    while (n--) {
        cin >> a;
        ans = max(query(root, a), ans);
        insert(root, a);
    }
    cout << ans << endl;
    return 0;
}
```

