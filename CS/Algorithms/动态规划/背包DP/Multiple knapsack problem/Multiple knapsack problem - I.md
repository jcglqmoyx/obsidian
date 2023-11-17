## [AcWing **4. 多重背包问题 I**](https://www.acwing.com/problem/content/description/4/)

有 N 种物品和一个容量是 V 的背包。

第 i 种物品最多有 $s_i$ 件，每件体积是 $v_i$，价值是 $w_i$。

求解将哪些物品装入背包，可使物品体积总和不超过背包容量，且价值总和最大。输出最大价值。

### **输入格式**

第一行两个整数 N，V，用空格隔开，分别表示物品种数和背包容积。

接下来有 N 行，每行三个整数 $v_i, w_i, s_i$，用空格隔开，分别表示第 i 种物品的体积、价值和数量。

### **输出格式**

输出一个整数，表示最大价值。

### **数据范围**

0 < N, V ≤ 100

0 < $v_i,w_i,s_i$ ≤ 100

### **输入样例**

```
4 5
1 2 3
2 4 1
3 4 3
4 5 2
```

### **输出样例：**

```
10
```

### O(N*V) 空间代码

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 105;
int n, m;
int v[N], w[N], s[N];
int f[N][N];

int main() {
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        cin >> v[i] >> w[i] >> s[i];
    }
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            for (int k = 0; k <= s[i] && k * v[i] <= j; k++) {
                f[i][j] = max(f[i][j], f[i - 1][j - v[i] * k] + w[i] * k);
            }
        }
    }
    cout << f[n][m] << endl;
    return 0;
}
```

### O(V）空间代码

```cpp
#include <bits/stdc++.h>

using namespace std;

int f[105], n, m;

int main() {
    cin >> n >> m;
    while (n--) {
        int v, w, s;
        cin >> v >> w >> s;
        while (s--) {
            for (int j = m; j >= v; j--) {
                f[j] = max(f[j], f[j - v] + w);
            }
        }
    }
    cout << f[m] << endl;
}
```