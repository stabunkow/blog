---
title: OJ-kaikeba-植物大战僵尸
date: 2021-05-21 10:00:04
tag: [OJ, kaikeba, 堆]
---

# 植物大战僵尸

![image-20210521104958520](https://hauk-blog.oss-cn-hangzhou.aliyuncs.com/blogimage-20210521104958520.png)

**样例输入**

```
2
3
10 2
9 3
8 4
6
1 1
2 2
3 3
4 4
5 5
6 1
```

**样例输出**

```
Case #1:
1 2 3
Case #2:
6 5 4 3 2 1
```

把僵尸按照速度分组，每个组建立一个堆，每次从堆顶中出一个。

```cpp
#include <bits/stdc++.h>

using namespace std;

struct zombie {
    int id;
    int f;
    friend bool operator < (zombie a, zombie b) {
        if (a.f == b.f) {
            // 优先队列中 id 大的先排后面
            return a.id > b.id;
        }
        return a.f < b.f;
    }
};

int main() {
    int t;
    cin >> t;
    int cs = 1;
    int n, f, s;
    while(cs <= t) {
        cin >> n;
        
        cout << "Case #" << cs << ":" << endl;
        // 相同恒定速度划为一组。
        priority_queue<zombie> d[110];
        for (int i = 1; i <= n; ++i) {
            cin >> f >> s;
            zombie z;
            z.id = i;
            z.f = f;
            d[s].push(z);
        }
        for (int i = 0; i < n; ++i) {
            int max_run = 0;
            int max_qid = -1;
            for (int j = 1; j <= 100; ++j) {
                if (!d[j].empty()) {
                    zombie z = d[j].top();
                    int run = z.f + i * j;
                    if (run > max_run) {
                        max_run = run;
                        max_qid = j;
                    } else if (run == max_run && max_qid != -1) {
                        zombie a = d[j].top();
                        zombie b = d[max_qid].top();
                        if (a.id < b.id) {
                            max_qid = j;
                        }
                    }
                }
            }
            cout << d[max_qid].top().id;
            d[max_qid].pop();
            if (i != n - 1) cout << " ";
        }
        cout << endl;
        cs++;
    }
    
    return 0;
}
```

