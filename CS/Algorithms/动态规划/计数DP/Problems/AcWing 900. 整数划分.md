## [AcWing 900. 整数划分](https://www.acwing.com/problem/content/description/902/)

一个正整数 n 可以表示成若干个正整数之和，形如：n = $n_1$ + $n_2$ + … + $n_k$，其中 $n_1$ ≥ $n_2$ ≥ … ≥ $n_k$, k ≥ 1。

我们将这样的一种表示称为正整数 n 的一种划分。

现在给定一个正整数 n，请你求出 n 共有多少种不同的划分方法。

### **输入格式**

共一行，包含一个整数 n。

### **输出格式**

共一行，包含一个整数，表示总划分数量。

由于答案可能很大，输出结果请对 $10^9+7$ 取模。

### **数据范围**

1 ≤ n ≤ 1000

### **输入样例:**

```
5
```

### **输出样例：**

```
7
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1005, MOD = 1e9 + 7;

int n;
int f[N];

int main() {
    cin >> n;
    f[0] = 1;
    for (int i = 1; i <= n; i++) {
        for (int j = i; j <= n; j++) {
            f[j] = (f[j] + f[j - i]) % MOD;
        }
    }
    cout << f[n] << endl;
    return 0;
}
```