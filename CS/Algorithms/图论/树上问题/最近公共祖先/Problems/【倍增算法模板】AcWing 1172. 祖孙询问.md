给定一棵包含 n 个节点的有根无向树，节点编号互不相同，但不一定是 1∼n。

有 m 个询问，每个询问给出了一对节点的编号 x 和 y，询问 x 与 y 的祖孙关系。

#### 输入格式

输入第一行包括一个整数 表示节点个数；

接下来 n 行每行一对整数 a 和 b，表示 a 和 b 之间有一条无向边。如果 b 是 −1，那么 a 就是树的根；

第 n+2 行是一个整数 m 表示询问个数；

接下来 m 行，每行两个不同的正整数 x 和 y，表示一个询问。

#### 输出格式

对于每一个询问，若 x 是 y 的祖先则输出 1，若 y 是 x 的祖先则输出 2，否则输出 0。

#### 数据范围

$1≤n,m≤4×10^4$
$1≤每个节点的编号≤4×10^4$
#### 输入样例：

```
10
234 -1
12 234
13 234
14 234
15 234
16 234
17 234
18 234
19 234
233 19
5
234 233
233 12
233 13
233 15
233 19
```

#### 输出样例：

```
1
0
0
0
2
```

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
const int N = 40010, M = N << 1, K = 16;  
  
int n, m;  
int d[N];  
int q[N];  
int p[N][19];  
  
int h[N], e[M], ne[M], idx;  
  
void add(int a, int b) {  
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;  
}  
  
void bfs(int root) {  
    memset(d, 0x3f, sizeof d);  
    d[root] = 0;  
    int hh = 0, tt = 0;  
    q[0] = root;  
    while (hh <= tt) {  
        int t = q[hh++];  
        for (int i = h[t]; ~i; i = ne[i]) {  
            int j = e[i];  
            if (d[j] > d[t] + 1) {  
                d[j] = d[t] + 1;  
                q[++tt] = j;  
                p[j][0] = t;  
                for (int k = 1; k < K; k++) {  
                    int anc = p[p[j][k - 1]][k - 1];  
                    if (t) p[j][k] = anc;  
                    else break;  
                }  
            }  
        }  
    }  
}  
  
int lca(int a, int b) {  
    if (d[a] > d[b]) swap(a, b);  
    for (int i = K - 1; i >= 0; i--) {  
        if (d[b] - (1 << i) >= d[a]) {  
            b = p[b][i];  
        }  
    }  
    if (a == b) return a;  
    for (int i = K - 1; i >= 0; i--) {  
        if (p[a][i] != p[b][i]) {  
            a = p[a][i];  
            b = p[b][i];  
        }  
    }  
    return p[a][0];  
}  
  
void solve() {  
    scanf("%d", &n);  
    memset(h, -1, sizeof h);  
    int root;  
    for (int i = 0; i < n; i++) {  
        int a, b;  
        scanf("%d%d", &a, &b);  
        if (b == -1) root = a;  
        else add(a, b), add(b, a);  
    }  
    bfs(root);  
    scanf("%d", &m);  
    while (m--) {  
        int x, y;  
        scanf("%d%d", &x, &y);  
        int anc = lca(x, y);  
        if (anc == x) puts("1");  
        else if (anc == y) puts("2");  
        else puts("0");  
    }  
}  
  
int main() {  
    solve();  
    return 0;  
}
```

预处理: $N\times logN$
单次查询两个节点a和b的最近公共祖先: $logN$
其中N为树中的节点数量。

Tarjan (时间复杂度为$O(N)$, 是离线算法)