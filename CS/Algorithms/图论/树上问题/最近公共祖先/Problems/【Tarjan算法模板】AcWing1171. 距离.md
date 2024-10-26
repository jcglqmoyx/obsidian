### 问题描述

给出 \(n\) 个点构成的一棵树，并有多次询问要求两点之间的最短距离。  

- **树的边是无向的**。
- 所有节点的编号是从 1 到 n。

---

### 输入格式

- **第一行**：两个整数 n 和 m。  
  - n 表示节点的数量。
  - m 表示询问的次数。

- **接下来的 n-1 行**：每行三个整数 x, y, k，表示点 x 和点 y 之间存在一条长度为 k 的边。

- **接下来的 m 行**：每行两个整数 x, y，表示询问点 x 到点 y 的最短距离。

---

### 输出格式

共 m 行，每行输出一个查询结果，对应每次询问的最短距离。

---

### 数据范围
$2 \leq n \leq 10^4$
$1 \leq m \leq 2 \times 10^4$
$0 < k \leq 100$
$1 \leq x, y \leq n$ 

---

### 输入样例 1

```
2 2 
1 2 100 
1 2 
2 1
```

### 输出样例1：
```
100
100
```

### 输入样例2：

```
3 2
1 2 10
3 1 15
1 2
3 2
```

### 输出样例2：

```
10
25
```

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
const int N = 10010, M = N << 1;  
  
int n, m;  
int d[N];  
int p[N];  
char st[N];  
int res[M];  
vector<vector<pair<int, int>>> query;  
  
int h[N], w[M], e[M], ne[M], idx;  
  
void add(int a, int b, int c) {  
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx++;  
}  
  
int find(int x) {  
    if (p[x] != x) p[x] = find(p[x]);  
    return p[x];  
}  
  
void dfs(int u, int parent) {  
    for (int i = h[u]; ~i; i = ne[i]) {  
        int j = e[i];  
        if (j == parent) continue;  
        d[j] = d[u] + w[i];  
        dfs(j, u);  
    }  
}  
  
void tarjan(int u, int parent) {  
    st[u] = 1;  
    for (int i = h[u]; ~i; i = ne[i]) {  
        int j = e[i];  
        if (j == parent) continue;  
        tarjan(j, u);  
        p[j] = u;  
    }  
    for (auto &[y, id]: query[u]) {  
        if (st[y] == 2) {  
            res[id] = d[y] + d[u] - d[find(y)] * 2;  
        }  
    }  
    st[u] = 2;  
}  
  
void solve() {  
    scanf("%d%d", &n, &m);  
    memset(h, -1, sizeof(h[0]) * (n + 1));  
    for (int i = 0; i + 1 < n; i++) {  
        int x, y, k;  
        scanf("%d%d%d", &x, &y, &k);  
        add(x, y, k), add(y, x, k);  
    }  
    query.resize(n + 1);  
    for (int i = 0; i < m; i++) {  
        int x, y;  
        scanf("%d%d", &x, &y);  
        query[x].emplace_back(y, i);  
        query[y].emplace_back(x, i);  
    }  
    dfs(1, -1);  
    iota(p + 1, p + n + 1, 1);  
    tarjan(1, -1);  
    for (int i = 0; i < m; i++) {  
        printf("%d\n", res[i]);  
    }  
}  
  
int main() {  
    solve();  
    return 0;  
}
```
# Tarjan算法为**离线算法**， 时间复杂度为O(N), 其中 N 为树中节点数量。