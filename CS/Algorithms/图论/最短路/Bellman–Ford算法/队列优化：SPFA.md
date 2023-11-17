SPFA算法求最短路一般时间复杂度为O(E), 最差时间复杂度为O(E * V)，最差的时候，会被卡成Bellman-Ford算法。

## [AcWing **851. SPFA求最短路**](https://www.acwing.com/problem/content/853/)

给定一个 n 个点 m 条边的有向图，图中可能存在重边和自环， **边权可能为负数**。

请你求出 1 号点到 n 号点的最短距离，如果无法从 1 号点走到 n 号点，则输出 `impossible`。

数据保证不存在负权回路。

### **输入格式**

第一行包含整数 n 和 m。

接下来 m 行每行包含三个整数 x, y, z，表示存在一条从点 x 到点 y 的有向边，边长为 z。

### **输出格式**

输出一个整数，表示 1 号点到 n 号点的最短距离。

如果路径不存在，则输出 `impossible`。

### **数据范围**

$1$ ≤ $n, m$ ≤ $10^5$,

图中涉及边长绝对值均不超过 10000。

### **输入样例：**

```
3 3
1 2 5
2 3 -3
1 3 4
```

### **输出样例：**

```
2
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1e5 + 5, INF = 0x3f3f3f3f;

int n, m;
bool st[N];
int dist[N];
int h[N], w[N], e[N], ne[N], idx;

void add(int a, int b, int c) {
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx++;
}

int spfa() {
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    queue<int> q;
    q.push(1);
    while (!q.empty()) {
        int t = q.front();
        q.pop();
        st[t] = false;
        for (int i = h[t]; ~i; i = ne[i]) {
            int j = e[i];
            if (dist[j] > dist[t] + w[i]) {
                dist[j] = dist[t] + w[i];
                if (!st[j]) {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }
    return dist[n];
}

int main() {
    memset(h, -1, sizeof h);
    scanf("%d%d", &n, &m);
    while (m--) {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c);
    }
    if (spfa() == INF) cout << "impossible" << endl;
    else cout << dist[n] << endl;
    return 0;
}
```

## [AcWing 852. SPFA判断负环](https://www.acwing.com/problem/content/854/)

给定一个 n 个点 m 条边的有向图，图中可能存在重边和自环， **边权可能为负数**。

请你判断图中是否存在负权回路。

### **输入格式**

第一行包含整数 n 和 m。

接下来 m 行每行包含三个整数 x, y, z，表示存在一条从点 x 到点 y 的有向边，边长为 z。

### **输出格式**

如果图中**存在**负权回路，则输出 `Yes`，否则输出 `No`。

### **数据范围**

1 ≤ n ≤ 2000，

1 ≤ m ≤ 10000，

图中涉及边长绝对值均不超过 10000。

### **输入样例：**

```
3 3
1 2 -1
2 3 4
3 1 -4
```

### **输出样例：**

```
Yes
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 10005;

int n, m;
int dist[N], cnt[N];
bool st[N];
int h[N], w[N], e[N], ne[N], idx;

void add(int a, int b, int c) {
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx++;
}

bool spfa() {
    queue<int> q;
    // 本题要求的是图中是否有负环，出发点不一定是1号点，所以把所有点都加入队列中；如果出发点为1号点，则只把1号点加入队列中
    for (int i = 1; i <= n; i++) {
        q.push(i);
        st[i] = true;
    }
    while (!q.empty()) {
        int t = q.front();
        q.pop();
        st[t] = false;
        for (int i = h[t]; i != -1; i = ne[i]) {
            int j = e[i];
            if (dist[j] > dist[t] + w[i]) {
                dist[j] = dist[t] + w[i];
                cnt[j] = cnt[t] + 1;
                if (cnt[j] >= n) return true;
                if (!st[j]) {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }
    return false;
}

int main() {
    memset(h, -1, sizeof h);
    scanf("%d%d", &n, &m);
    while (m--) {
        int x, y, z;
        scanf("%d%d%d", &x, &y, &z);
        add(x, y, z);
    }
    if (spfa()) puts("Yes");
    else puts("No");
    return 0;
}
```