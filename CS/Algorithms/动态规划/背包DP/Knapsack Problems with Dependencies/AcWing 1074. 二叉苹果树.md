有一棵二叉苹果树，如果树枝有分叉，一定是分两叉，即没有只有一个儿子的节点。

这棵树共 N个节点，编号为 1 至 N，树根编号一定为 1。

我们用一根树枝两端连接的节点编号描述一根树枝的位置。

一棵苹果树的树枝太多了，需要剪枝。但是一些树枝上长有苹果，给定需要保留的树枝数量，求最多能留住多少苹果。

这里的保留是指最终与1号点连通。

#### 输入格式

第一行包含两个整数 N和 Q，分别表示树的节点数以及要保留的树枝数量。

接下来 N−1 行描述树枝信息，每行三个整数，前两个是它连接的节点的编号，第三个数是这根树枝上苹果数量。

#### 输出格式

输出仅一行，表示最多能留住的苹果的数量。

#### 数据范围

1≤Q<N≤100.
N≠1,  
每根树枝上苹果不超过 30000 个。

#### 输入样例：

```
5 2
1 3 1
1 4 10
2 3 20
3 5 20
```

#### 输出样例：

```
21
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 105, M = N << 1;

int n, m;
int f[N][N];

int h[N], w[M], e[M], ne[M], idx;

void add(int a, int b, int c) {
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx++;
}

void dfs(int u, int p) {
    for (int i = h[u]; ~i; i = ne[i]) {
        if (e[i] == p) continue;
        dfs(e[i], u);
        for (int j = m; j; j--) {
            for (int k = 0; k < j; k++) {
                f[u][j] = max(f[u][j], f[u][j - k - 1] + f[e[i]][k] + w[i]);
            }
        }
    }
}

void solve() {
    cin >> n >> m;
    memset(h, -1, sizeof(int) * (n + 1));
    for (int i = 0; i < n - 1; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c), add(b, a, c);
    }
    dfs(1, 0);
    cout << f[1][m] << endl;
}

int main() {
    solve();
    return 0;
}
```