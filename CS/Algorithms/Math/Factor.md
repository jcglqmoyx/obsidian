## [AcWing **870. 约数个数**](https://www.acwing.com/problem/content/872/)

给定 n 个正整数 $a_i$，请你输出这些数的乘积的约数个数，答案对 $10^9$+7 取模。

### **输入格式**

第一行包含整数 n。

接下来 n 行，每行包含一个整数 $a_i$。

### **输出格式**

输出一个整数，表示所给正整数的乘积的约数个数，答案需对 $10^9$+7 取模。

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
12
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int MOD = 1e9 + 7;

int solve(int n) {
    unordered_map<int, int> cnt;
    while (n--) {
        int a;
        cin >> a;
        for (int i = 2; i <= a / i; i++) {
            while (a % i == 0) {
                cnt[i]++;
                a /= i;
            }
        }
        if (a > 1) cnt[a]++;
    }
    long long res = 1;
    for (auto &x: cnt) {
        res = res * (x.second + 1) % MOD;
    }
    return (int) res;
}

int main() {
    int n;
    cin >> n;
    cout << solve(n) << endl;
    return 0;
}
```

## [AcWing **871. 约数之和**](https://www.acwing.com/problem/content/description/873/)

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

## [AcWing 872. 最大公约数](https://www.acwing.com/problem/content/874/)

给定 n 对正整数 $a_i$, $b_i$，请你求出每对数的最大公约数。

### **输入格式**

第一行包含整数 n。

接下来 n 行，每行包含一个整数对 $a_i$, $b_i$。

### **输出格式**

输出共 n 行，每行输出一个整数对的最大公约数。

### **数据范围**

1 ≤ n ≤ $10^5$，

1 ≤ $a_i$，$b_i$ ≤ 2 * $10^9$

### **输入样例：**

```
2
3 6
4 6
```

### **输出样例：**

```
3
2
```

```cpp
#include <bits/stdc++.h>

using namespace std;

int gcd(int m, int n) {
    return m % n == 0 ? n : gcd(n, m % n);
}

int main() {
    int n;
    cin >> n;
    while (n--) {
        int a, b;
        cin >> a >> b;
        cout << gcd(a, b) << endl;
    }
    return 0;
}
```