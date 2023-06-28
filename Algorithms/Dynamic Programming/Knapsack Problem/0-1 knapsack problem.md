# [AcWing 2. 01背包问题](https://www.acwing.com/problem/content/2/)

有 $N$ 件物品和一个容量是 $V$ 的背包。每件物品只能使用一次。

第 $i$ 件物品的体积是 $a_i$，价值是 $w_i$。

求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。输出最大价值。

### **输入格式**

第一行两个整数，$N$，$V$，用空格隔开，分别表示物品数量和背包容积。

接下来有 $N$ 行，每行两个整数 $v_i$，$w_i$，用空格隔开，分别表示第 $i$ 件物品的体积和价值。

### **输出格式**

输出一个整数，表示最大价值。

### **数据范围**

$0 < N, V \le 1000$

$0 < v_i, w_i \le 1000$

### **输入样例**

```
4 5
1 2
2 4
3 4
4 5
```

### **输出样例：**

```
8
```

---

## 思路:

用$f[i][j]$表示前 $i$ 个物品，背包容量 $j$ 的情况下的最优解（可获得的最大价值），则$f[i][j] = max(f[i - 1][j], \ f[i - 1][j - v[i]] + w[i]）$，时间复杂度为$O(N*V)$，空间复杂度可以优化到$O(V)$。

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1005;

int n, m;
int f[N][N];
int v[N], w[N];

int main() {
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        cin >> v[i] >> w[i];
    }
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            f[i][j] = f[i - 1][j];
            if (j >= v[i]) f[i][j] = max(f[i][j], f[i - 1][j - v[i]] + w[i]);
        }
    }
    cout << f[n][m] << endl;
    return 0;
}
```

状态数组$f$可以优化为一维:

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1005;

int f[N];
int v[N], w[N];
int n, m;

int main() {
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        cin >> v[i] >> w[i];
    }
    for (int i = 1; i <= n; i++) {
        for (int j = m; j >= v[i]; j--) {
            f[j] = max(f[j], f[j - v[i]] + w[i]);
        }
    }
    cout << f[m] << endl;
    return 0;
}
```

我们注意到在处理数据时，我们是一个物品一个物品，一个一个体积的枚举。因此我们可以不必开两个数组记录体积和价值，而是边输入边处理。

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1005;

int f[N];

int main() {
    int n, m;
    cin >> n >> m;

    for (int i = 1; i <= n; i++) {
        int v, w;
        cin >> v >> w;
        for (int j = m; j >= v; j--) {
            f[j] = max(f[j], f[j - v] + w);
        }
    }
    cout << f[m] << endl;
    return 0;
}
```