## [AcWing **875. 快速幂**](https://www.acwing.com/problem/content/877/)

给定 n 组 $a_i$, $b_i$, $p_i$，对于每组数据，求出 $a_i^{b_i}mod\ p_i$ 的值。

### **输入格式**

第一行包含整数 n。

接下来 n 行，每行包含三个整数 $a_i, b_i, p_i$。

### **输出格式**

对于每组数据，输出一个结果，表示  $a_i^{b_i}\mod\ p_i$ 的值。

每个结果占一行。

### **数据范围**

1 ≤ n ≤ 100000，

1 ≤ $a_i$, $b_i$, $p_i$ ≤ $2 * 10^9$

### **输入样例：**

```
2
3 2 5
4 3 9
```

### **输出样例：**

```
4
1
```

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;

ll quick_pow(ll a, ll b, int p) {
    ll res = 1;
    while (b) {
        if (b & 1) res = res * a % p;
        a = a * a % p;
        b >>= 1;
    }
    return res;
}

int main() {
    int n;
    cin >> n;
    while (n--) {
        int a, b, p;
        cin >> a >> b >> p;
        cout << quick_pow(a, b, p) << endl;
    }
    return 0;
}
```

## [AcWing **876. 快速幂求逆元**](https://www.acwing.com/problem/content/878/)

给定 n 组 $a_i, b_i, p_i$，其中 $p_i$是质数，求 $a_i$ 模 $p_i$ 的乘法逆元，若逆元不存在则输出 `impossible`。

**注意**：请返回在 0 ~ p - 1 之间的逆元。

### **输入格式**

第一行包含整数 n。

接下来 n 行，每行包含一个数组 $a_i, b_i, p_i$，数据保证 $p_i$ 是质数。

### **输出格式**

输出共 n 行，每组数据输出一个结果，每个结果占一行。

若 $a_i$ 模 $p_i$ 的乘法逆元存在，则输出一个整数，表示逆元，否则输出 `impossible`。

### **数据范围**

1 ≤ n ≤ $10^5$，

1 ≤ $a_i$，$p_i$ ≤ $2*10^9$

### **输入样例：**

```
3
4 3
8 5
6 3
```

### **输出样例：**

```
1
2
impossible
```

```cpp
#include <bits/stdc++.h>

using namespace std;

using LL = long long;

LL qmi(int a, int b, int p) {
    LL res = 1;
    while (b) {
        if (b & 1) res = res * a % p;
        a = a * (LL) a % p;
        b >>= 1;
    }
    return res;
}

int main() {
    int n;
    scanf("%d", &n);
    while (n--) {
        int a, p;
        scanf("%d%d", &a, &p);
        if (a % p == 0) puts("impossible");
        else printf("%lld\n", qmi(a, p - 2, p));
    }
    return 0;
}
```