Time complexity: O(V * E)

Space complexity: O(V)

## AcWing **853. 有边数限制的最短路**

给定一个 n 个点 mm 条边的有向图，图中可能存在重边和自环， **边权可能为负数**。

请你求出从 1 号点到 n 号点的最多经过 kk 条边的最短距离，如果无法从 1 号点走到 n 号点，输出 `impossible`。

注意：图中可能 **存在负权回路** 。

### **输入格式**

第一行包含三个整数 n, m, k。

接下来 m 行，每行包含三个整数 x, y, z，表示存在一条从点 x 到点 y 的有向边，边长为 z。

点的编号为 1 ∼ n。

### **输出格式**

输出一个整数，表示从 11 号点到 nn 号点的最多经过 kk 条边的最短距离。

如果不存在满足条件的路径，则输出 `impossible`。

### **数据范围**

1 ≤ n, k ≤ 500，

1 ≤ m ≤ 10000，

1 ≤ x, y ≤n，

任意边长的绝对值不超过 10000。

### **输入样例：**

```
3 3 1
1 2 1
2 3 1
1 3 3
```

### **输出样例：**

```
3
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 505, M = 10005, INF = 0x3f3f3f3f;

int n, m, k;
int dist[N], last[N];

struct Edge {
    int a, b, w;
} edges[M];

void bellman_ford() {
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    for (int i = 0; i < k; i++) {
        memcpy(last, dist, sizeof dist);
        for (int j = 0; j < m; j++) {
            auto e = edges[j];
            dist[e.b] = min(dist[e.b], last[e.a] + e.w);
        }
    }
}

int main() {
    scanf("%d%d%d\n", &n, &m, &k);
    for (int i = 0; i < m; i++) {
        int x, y, z;
        cin >> x >> y >> z;
        edges[i] = {x, y, z};
    }
    bellman_ford();
    if (dist[n] > INF / 2) puts("impossible");
    else printf("%d\n", dist[n]);
    return 0;
}
```