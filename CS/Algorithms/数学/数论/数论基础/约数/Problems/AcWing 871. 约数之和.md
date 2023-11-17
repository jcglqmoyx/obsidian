## [AcWing 871. 约数之和](https://www.acwing.com/problem/content/description/873/)

给定 n 个正整数 $a_i$，请你输出这些数的乘积的约数之和，答案对 $10^9$+7 取模。

### **输入格式**

第一行包含整数 n。

接下来 n 行，每行包含一个整数 $a_i$。

### **输出格式**

输出一个整数，表示所给正整数的乘积的约数之和，答案需对 $10^9$+7  取模。

### **数据范围**

1 ≤ n ≤ 100，

1 ≤ $a_i$ ≤ 2 * $10^9$

### **输入样例：**

```
3
2
6
8
```

### **输出样例：**

```
252
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int MOD = 1e9 + 7;

int main() {
    int n;
    cin >> n;
    unordered_map<int, int> primes;
    while (n--) {
        int x;
        cin >> x;
        for (int i = 2; i * i <= x; i++) {
            while (x % i == 0) {
                primes[i]++;
                x /= i;
            }
        }
        if (x > 1) primes[x]++;
    }
    long long res = 1;
    for (auto &x: primes) {
        int base = x.first, exponent = x.second;
        long long t = 1;
        while (exponent--) {
            t = (base * t + 1) % MOD;
        }
        res = res * t % MOD;
    }
    cout << res << endl;
    return 0;
}
```