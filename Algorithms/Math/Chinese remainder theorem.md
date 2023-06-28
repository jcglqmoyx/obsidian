# Chinese remainder theorem

[See Wikipedia](https://en.wikipedia.org/wiki/Chinese_remainder_theorem)

## [AcWing 204. 表达整数的奇怪方式](https://www.acwing.com/problem/content/206/)

给定 2n 个整数 $a_1, a_2, ..\ a_n$ 和 $m_1, m_2, ..\ m_n$，求一个最小的非负整数 x，

满足 $∀_i$∈[1, n], x ≡ $m_i$(mod $a_i$)。

### **输入格式**

第 1 行包含整数 n。

第 2 … n + 1行：每 i + 1 行包含两个整数 $a_i$ 和 $m_i$，数之间用空格隔开。

### **输出格式**

输出最小非负整数 x，如果 x 不存在，则输出 −1。如果存在 x，则数据保证 x 一定在 64 位整数范围内。

### **数据范围**

1 ≤ $a_i$ ≤ $2^{31}$ - 1，

0 ≤ m ≤ $a_i$，

1 ≤ n ≤ 25

### **输入样例：**

```
2
8 7
11 9
```

### **输出样例：**

```
31
```

```cpp
#include <bits/stdc++.h>

using namespace std;

using LL = long long;

LL exgcd(LL a, LL b, LL &x, LL &y) {
    if (!b) {
        x = 1, y = 0;
        return a;
    }

    LL d = exgcd(b, a % b, y, x);
    y -= a / b * x;
    return d;
}

int main() {
    int n;
    cin >> n;

    LL x = 0, m1, a1;
    cin >> m1 >> a1;
    for (int i = 0; i < n - 1; i++) {
        LL m2, a2;
        cin >> m2 >> a2;
        LL k1, k2;
        LL d = exgcd(m1, m2, k1, k2);
        if ((a2 - a1) % d) {
            x = -1;
            break;
        }

        k1 *= (a2 - a1) / d;
        k1 = (k1 % (m2 / d) + m2 / d) % (m2 / d);

        x = k1 * m1 + a1;

        LL m = abs(m1 / d * m2);
        a1 = k1 * m1 + a1;
        m1 = m;
    }

    if (x != -1) x = (a1 % m1 + m1) % m1;

    cout << x << endl;

    return 0;
}
```