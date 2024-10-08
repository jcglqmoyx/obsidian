朴素版Prim算法求MST时间复杂度为O($V^2$)，适用于稠密图；对于稀疏图，可以使用堆优化，时间复杂度为O(E*logV)，但是处理稀疏图问题，更多的是使用Kruskal算法。

## [AcWing 858](https://www.acwing.com/problem/content/description/860/)

给定一个 n 个点 m 条边的无向图，图中可能存在重边和自环，边权可能为负数。

求最小生成树的树边权重之和，如果最小生成树不存在则输出 `impossible`。

给定一张边带权的无向图 G = (V, E)，其中 V 表示图中点的集合，E 表示图中边的集合，n = |V|，m = |E|。

由 V 中的全部 n 个顶点和 E 中 n−1 条边构成的无向连通子图被称为 G 的一棵生成树，其中边的权值之和最小的生成树被称为无向图 G 的最小生成树。

### **输入格式**

第一行包含两个整数 n 和 m。

接下来 m 行，每行包含三个整数 u, v, w，表示点 u 和点 v 之间存在一条权值为 w 的边。

### **输出格式**

共一行，若存在最小生成树，则输出一个整数，表示最小生成树的树边权重之和，如果最小生成树不存在则输出 `impossible`。

### **数据范围**

1 ≤ n ≤ 500，

1 ≤ m ≤ $10^5$，

图中涉及边的边权的绝对值均不超过 10000。

### **输入样例：**

```
4 5
1 2 1
1 3 2
1 4 3
2 3 2
3 4 4
```

### **输出样例：**

```
6
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 510, INF = 0x3f3f3f3f;

int n, m;
int g[N][N];
int dist[N];
bool st[N];

int prim() {
    memset(dist, 0x3f, sizeof dist);

    int res = 0;
    for (int i = 0; i < n; i++) {
        int t = -1;
        for (int j = 1; j <= n; j++) {
            if (!st[j] && (t == -1 || dist[t] > dist[j])) t = j;
        }

        if (i && dist[t] == INF) return INF;

        if (i) res += dist[t];
        st[t] = true;

        for (int j = 1; j <= n; j++) dist[j] = min(dist[j], g[t][j]);
    }

    return res;
}

int main() {
    scanf("%d%d", &n, &m);

    memset(g, 0x3f, sizeof g);

    while (m--) {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        g[a][b] = g[b][a] = min(g[a][b], c);
    }

    int t = prim();

    if (t == INF) puts("impossible");
    else printf("%d\n", t);

    return 0;
}
```