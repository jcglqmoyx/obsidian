# Euler's totient function

[See Wikipedia](Euler's%20totient%20function.md)

**Euler's totient function** (also called the Phi function) counts the number of positive integers less than n that are coprime to n.

Euler's totient function is **a multiplicative function, meaning that if two numbers m and n are relatively prime, then φ(mn) = φ(m)φ(n).**

## [AcWing **873. 欧拉函数**](https://www.acwing.com/problem/content/description/875/)

给定 n 个正整数 $a_i$，请你求出每个数的欧拉函数。

### **输入格式**

第一行包含整数 n。

接下来 n 行，每行包含一个正整数 $a_i$。

### **输出格式**

输出共 n 行，每行输出一个正整数 $a_i$ 的欧拉函数。

### **数据范围**

1 ≤ n ≤ 100，

1 ≤ $a_i$ ≤ 2 * $10^9$

### **输入样例：**

```
3
3
6
8
```

### **输出样例：**

```
2
2
4
```

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
    int n;
    cin >> n;
    while (n--) {
        int a;
        cin >> a;
        long long res = a;
        for (int i = 2; i * i <= a; i++) {
            if (a % i == 0) {
                res = res * (i - 1) / i;
                while (a % i == 0) {
                    a /= i;
                }
            }
        }
        if (a > 1) res = res * (a - 1) / a;
        cout << res << endl;
    }
    return 0;
}
```

## [AcWing **874. 筛法求欧拉函数**](https://www.acwing.com/problem/content/876/)

给定一个正整数 n，求 1 ∼ n 中每个数的欧拉函数之和。

### **输入格式**

共一行，包含一个整数 n。

### **输出格式**

共一行，包含一个整数，表示 1 ∼ n 中每个数的欧拉函数之和。

### **数据范围**

1 ≤ n ≤ $10^6$

### **输入样例：**

```
6
```

### **输出样例：**

```
12
```

```cpp
#include <bits/stdc++.h>

using namespace std;

using LL = long long;

const int N = 1000010;

int primes[N], cnt;
int euler[N];
bool st[N];

void get_eulers(int n) {
    euler[1] = 1;
    for (int i = 2; i <= n; i++) {
        if (!st[i]) {
            primes[cnt++] = i;
            euler[i] = i - 1;
        }
        for (int j = 0; primes[j] * i <= n; j++) {
            int t = primes[j] * i;
            st[t] = true;
            if (i % primes[j] == 0) {
                euler[t] = euler[i] * primes[j];
                break;
            }
            euler[t] = euler[i] * (primes[j] - 1);
        }
    }
}

int main() {
    int n;
    cin >> n;

    get_eulers(n);

    LL res = 0;
    for (int i = 1; i <= n; i++) res += euler[i];

    cout << res << endl;

    return 0;
}
```