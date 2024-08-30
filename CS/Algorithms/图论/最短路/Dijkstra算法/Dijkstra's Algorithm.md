## [AcWing 849 (朴素版Dijkstra, 稠密图,](https://www.acwing.com/problem/content/description/851/) O($V^2$))

给定一个 n 个点 m 条边的有向图，图中可能存在重边和自环，所有边权均为正值。

请你求出 1 号点到 n 号点的最短距离，如果无法从 1 号点走到 n 号点，则输出 −1。

### **输入格式**

第一行包含整数 n 和 m。

接下来 m 行每行包含三个整数 x, y, z，表示存在一条从点 x 到点 y 的有向边，边长为 z。

### **输出格式**

输出一个整数，表示 1 号点到 n 号点的最短距离。

如果路径不存在，则输出 −1。

### **数据范围**

1 ≤ n ≤ 500,

1 ≤ m ≤ $10^5$,

图中涉及边长均不超过10000。

### **输入样例：**

```
3 3
1 2 2
2 3 1
1 3 4
```

### **输出样例：**

```
3
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 510, INF = 0x3f3f3f3f;

int n, m;
int g[N][N];
int dist[N];
bool st[N];

int dijkstra() {
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;

    for (int i = 0; i < n - 1; i++) {
        int t = -1;
        for (int j = 1; j <= n; j++) {
            if (!st[j] && (t == -1 || dist[j] < dist[t])) t = j;
        }
        for (int j = 1; j <= n; j++) {
            if (!st[j]) dist[j] = min(dist[j], dist[t] + g[t][j]);
        }
        st[t] = true;
    }
    if (dist[n] == INF) return -1;
    return dist[n];
}

int main() {
    scanf("%d%d", &n, &m);

    memset(g, 0x3f, sizeof g);
    while (m--) {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        g[a][b] = min(g[a][b], c);
    }
    printf("%d\n", dijkstra());
    return 0;
}
```

## [AcWing 850 (堆优化版Dijkstra, 稀疏图, O((V+E) * log(V))](https://www.acwing.com/problem/content/description/852/)

给定一个 n 个点 m 条边的有向图，图中可能存在重边和自环，所有边权均为非负值。

请你求出 1 号点到 n 号点的最短距离，如果无法从 1 号点走到 n 号点，则输出 −1。

### **输入格式**

第一行包含整数 n 和 m。

接下来 mm 行每行包含三个整数 x, y, z，表示存在一条从点 x 到点 y 的有向边，边长为 z。

### **输出格式**

输出一个整数，表示 1 号点到 n 号点的最短距离。

如果路径不存在，则输出 −1。

### **数据范围**

1 ≤ n, m ≤ $10^5$,

图中涉及边长均不小于 0，且不超过 10000。

数据保证：如果最短路存在，则最短路的长度不超过 $10^9$。

### **输入样例：**

```
3 3
1 2 2
2 3 1
1 3 4
```

### **输出样例：**

```
3
```

```cpp
#include <bits/stdc++.h>

using namespace std;

using PII = pair<int, int>;

const int N = 1500010, INF = 0x3f3f3f3f;

int n, m;
int h[N], w[N], e[N], ne[N], idx;

int dist[N];
bool st[N];

void add(int a, int b, int c) {
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx++;
}

int dijkstra() {
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    priority_queue<PII, vector<PII>, greater<>> heap;
    heap.push({0, 1});

    while (!heap.empty()) {
        auto t = heap.top();
        heap.pop();

        int p = t.second;

        if (st[p]) continue;
        st[p] = true;

        for (int i = h[p]; i != -1; i = ne[i]) {
            int j = e[i];
            if (dist[j] > dist[p] + w[i]) {
                dist[j] = dist[p] + w[i];
                heap.push({dist[j], j});
            }
        }
    }

    if (dist[n] == INF) return -1;
    return dist[n];
}

int main() {
    scanf("%d%d", &n, &m);

    memset(h, -1, sizeof h);
    while (m--) {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c);
    }
    printf("%d\n", dijkstra());
    return 0;
}
```