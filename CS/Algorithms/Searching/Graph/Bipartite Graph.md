## [AcWing **860. 染色法判定二分图](https://www.acwing.com/problem/content/862/) O(V + E)**

给定一个 n 个点 m 条边的无向图，图中可能存在重边和自环。

请你判断这个图是否是二分图。

### **输入格式**

第一行包含两个整数 n 和 m。

接下来 m 行，每行包含两个整数 u 和 v，表示点 u 和点 v 之间存在一条边。

### **输出格式**

如果给定图是二分图，则输出 `Yes`，否则输出 `No`。

### **数据范围**

1 ≤ n, m ≤ $10^5$

### **输入样例：**

```
4 4
1 3
1 4
2 3
2 4
```

### **输出样例：**

```
Yes
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 100010, M = N << 1;

int n, m;
int color[N];
int h[N], e[M], ne[M], idx;

void add(int a, int b) {
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

bool dfs(int u, int c) {
    color[u] = c;
    for (int i = h[u]; ~i; i = ne[i]) {
        int j = e[i];
        if (!color[j]) {
            if (!dfs(j, 3 - c)) {
                return false;
            }
        } else if (color[j] == c) {
            return false;
        }
    }
    return true;
}

int main() {
    memset(h, -1, sizeof h);
    scanf("%d%d", &n, &m);
    while (m--) {
        int x, y;
        scanf("%d%d", &x, &y);
        add(x, y), add(y, x);
    }
    bool flag = true;
    for (int i = 1; i <= n; i++) {
        if (!color[i]) {
            if (!dfs(i, 1)) {
                flag = false;
                break;
            }
        }
    }
    puts(flag ? "Yes" : "No");
    return 0;
}
```

## [AcWing 861. 二分图的最大匹配](https://www.acwing.com/problem/content/863/) O(V * E)

给定一个二分图，其中左半部包含 $n_1$ 个点（编号 1 ~ $n_1$），右半部包含 $n_2$ 个点（编号 1 ~ $n_2$），二分图共包含 $m$ 条边。

数据保证任意一条边的两个端点都不可能在同一部分中。

请你求出二分图的最大匹配数。

> 二分图的匹配：给定一个二分图 G，在 G 的一个子图 M 中，MM 的边集 {E} 中的任意两条边都不依附于同一个顶点，则称 M 是一个匹配。
> 
> 
> 二分图的最大匹配：所有匹配中包含边数最多的一组匹配被称为二分图的最大匹配，其边数即为最大匹配数。
> 

### **输入格式**

第一行包含三个整数 $n_1$、 $n_2$ 和 $m$。

接下来 m 行，每行包含两个整数 u 和 v，表示左半部点集中的点 u 和右半部点集中的点 v 之间存在一条边。

### **输出格式**

输出一个整数，表示二分图的最大匹配数。

### **数据范围**

1 ≤ $n_1$, $n_2$ ≤ 500，

1 ≤ u ≤ $n_1$，

1 ≤ v ≤ $n_2$，

1 ≤ m ≤ $10^5$

### **输入样例：**

```
2 2 4
1 1
1 2
2 1
2 2
```

### **输出样例：**

```
2
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 505, M = 100010;

int n1, n2, m;
int h[N], e[M], ne[M], idx;
int match[N];
bool st[N];

void add(int a, int b) {
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

bool find(int u) {
    for (int i = h[u]; ~i; i = ne[i]) {
        int j = e[i];
        if (!st[j]) {
            st[j] = true;
            if (!match[j] || find(match[j])) {
                match[j] = u;
                return true;
            }
        }
    }
    return false;
}

int main() {
    memset(h, -1, sizeof h);
    scanf("%d%d%d", &n1, &n2, &m);
    while (m--) {
        int x, y;
        scanf("%d%d", &x, &y);
        add(x, y);
    }
    int res = 0;
    for (int i = 1; i <= n1; i++) {
        memset(st, false, sizeof st);
        if (find(i)) res++;
    }
    printf("%d\n", res);
    return 0;
}
```