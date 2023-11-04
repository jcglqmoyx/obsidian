---
page-title: "164. 可达性统计 - AcWing题库"
url: https://www.acwing.com/problem/content/166/
date: "2023-11-04 11:29:17"
---
164\. 可达性统计

-     [题目](https://www.acwing.com/problem/content/description/166/)
-     [提交记录](https://www.acwing.com/problem/content/submission/166/)
-     [讨论](https://www.acwing.com/problem/content/discussion/index/166/1/)
-     [题解](https://www.acwing.com/problem/content/solution/166/1/)
-     [视频讲解](https://www.acwing.com/problem/content/video/166/)

  

给定一张 N 个点 M 条边的有向无环图，分别统计从每个点出发能够到达的点的数量。

#### 输入格式

第一行两个整数 N,M，接下来 M 行每行两个整数 x,y，表示从 x 到 y 的一条有向边。

#### 输出格式

输出共 N 行，表示每个点能够到达的点的数量。

#### 数据范围

1≤N,M≤30000,  
1≤x,y≤N

#### 输入样例：

```
10 10
3 8
2 3
2 5
5 9
5 9
2 3
3 9
4 8
2 10
4 9
```

#### 输出样例：

```
1
6
3
3
2
1
1
1
1
1
```

---

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 30010;

int n, m;
bitset<N> f[N];
int h[N], e[N], ne[N], idx;

void add(int a, int b) {
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

bitset<N> &dp(int u) {
    if (f[u].count()) return f[u];
    f[u][u] = true;
    for (int i = h[u]; ~i; i = ne[i]) {
        int j = e[i];
        f[u] |= dp(j);
    }
    return f[u];
}

void solve() {
    scanf("%d%d\n", &n, &m);
    memset(h, -1, sizeof(int) * (n + 1));
    int x, y;
    while (m--) {
        scanf("%d%d\n", &x, &y);
        add(x, y);
    }
    for (int i = 1; i <= n; i++) {
        printf("%zu\n", dp(i).count());
    }
}

int main() {
    solve();
    return 0;
}
```