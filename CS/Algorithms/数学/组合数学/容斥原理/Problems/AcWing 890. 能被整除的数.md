## [AcWing 890. 能被整除的数](https://www.acwing.com/problem/content/description/892/)

给定一个整数 n 和 m 个不同的质数 $p_1$，$p_2$ , … , $p_m$。

请你求出 1 ∼ n 中能被 $p_1$，$p_2$ , … , $p_m$ 中的至少一个数整除的整数有多少个。

### **输入格式**

第一行包含整数 n 和 m。

第二行包含 m 个质数。

### **输出格式**

输出一个整数，表示满足条件的整数的个数。

### **数据范围**

1 ≤ m ≤ 16，

1 ≤ n, $p_i$≤ $10^9$

### **输入样例：**

```
10 2
2 3
```

### **输出样例：**

```
7
```

```cpp
#include <bits/stdc++.h>

using namespace std;

using LL = long long;

const int N = 20;

int n, m;
int p[N];

int main() {
    cin >> n >> m;
    for (int i = 0; i < m; i++) cin >> p[i];
    int res = 0;
    for (int i = 1; i < 1 << m; i++) {
        int s = 0, t = 1;
        for (int j = 0; j < m; j++) {
            if (i >> j & 1) {
                if ((LL) t * p[j] > n) {
                    t = -1;
                    break;
                }
                s++;
                t *= p[j];
            }
        }
        if (t == -1) continue;
        if (s & 1) res += n / t;
        else res -= n / t;
    }
    cout << res << endl;
    return 0;
}
```

