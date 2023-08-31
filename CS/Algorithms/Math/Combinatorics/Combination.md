# Combination

## [AcWing **885. 求组合数 I**](https://www.acwing.com/problem/content/887/)

给定 n 组询问，每组询问给定两个整数 a，b，请你输出 $C_a^b$ mod ($10^9+7$) 的值。

### **输入格式**

第一行包含整数 n。

接下来 n 行，每行包含一组 a 和 b。

### **输出格式**

共 n 行，每行输出一个询问的解。

### **数据范围**

1 ≤ n ≤ 10000，

1 ≤ b ≤ a ≤ 2000

### **输入样例：**

```
3
3 1
5 3
2 2
```

### **输出样例：**

```
3
10
1
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 2005, mod = 1e9 + 7;

int c[N][N];

void init() {
    for (int i = 0; i < N; i++) {
        for (int j = 0; j <= i; j++) {
            if (!j) c[i][j] = 1;
            else c[i][j] = (c[i - 1][j] + c[i - 1][j - 1]) % mod;
        }
    }
}

int main() {
    int n;
    scanf("%d", &n);
    init();
    while (n--) {
        int i, j;
        scanf("%d%d", &i, &j);
        printf("%d\n", c[i][j]);
    }
    return 0;
}
```

## [AcWing **886. 求组合数 II**](https://www.acwing.com/problem/content/888/)

给定 n 组询问，每组询问给定两个整数 a，b，请你输出 $C_a^b$ mod ($10^9+7$) 的值。

### **输入格式**

第一行包含整数 n。

接下来 n 行，每行包含一组 a 和 b。

### **输出格式**

共 n 行，每行输出一个询问的解。

### **数据范围**

1 ≤ n ≤ 10000，

1 ≤ b ≤ a ≤ $10^5$

### **输入样例：**

```
3
3 1
5 3
2 2
```

### **输出样例：**

```cpp
3
10
1
```

```cpp
#include <bits/stdc++.h>

using namespace std;

using LL = long long;

const int N = 100010, mod = 1e9 + 7;

LL fact[N], infact[N];

LL quick_power(LL a, int k, int p) {
    LL res = 1;
    while (k) {
        if (k & 1) res = res * a % p;
        a = a * a % p;
        k >>= 1;
    }
    return res;
}

int main() {
    fact[0] = infact[0] = 1;
    for (int i = 1; i < N; i++) {
        fact[i] = fact[i - 1] * i % mod;
        infact[i] = infact[i - 1] * quick_power(i, mod - 2, mod) % mod;
    }

    int n;
    scanf("%d", &n);
    while (n--) {
        int a, b;
        scanf("%d%d", &a, &b);
        printf("%lld\n", fact[a] * infact[b] % mod * infact[a - b] % mod);
    }

    return 0;
}
```

## [AcWing **887. 求组合数 III**](https://www.acwing.com/problem/content/889/)

给定 n 组询问，每组询问给定三个整数 a, b, p，其中 p 是质数，请你输出 $C_a^b$ mod p 的值。

### **输入格式**

第一行包含整数 n。

接下来 n 行，每行包含一组 a, b, p。

### **输出格式**

共 n 行，每行输出一个询问的解。

### **数据范围**

1 ≤ n ≤ 20，

1 ≤ b ≤ a ≤ $10^{18}$，

1 ≤ p ≤ $10^5$。

### **输入样例：**

```
3
5 3 7
3 1 5
6 4 13
```

### **输出样例：**

```
3
3
2
```

```cpp
#include <bits/stdc++.h>

using namespace std;

using LL = long long;

LL quick_power(LL a, int k, int p) {
    LL res = 1;
    while (k) {
        if (k & 1) res = res * a % p;
        a = a * a % p;
        k >>= 1;
    }
    return res;
}

LL C(LL a, LL b, int p) {
    if (b > a) return 0;

    LL res = 1;
    for (int i = 1, j = a; i <= b; i++, j--) {
        res = res * j % p;
        res = res * quick_power(i, p - 2, p) % p;
    }
    return res;
}

LL lucas(LL a, LL b, int p) {
    if (a < p && b < p) return C(a, b, p);
    return C(a % p, b % p, p) * lucas(a / p, b / p, p) % p;
}

int main() {
    int n;
    scanf("%d", &n);

    while (n--) {
        LL a, b;
        int p;
        scanf("%lld%lld%d", &a, &b, &p);
        printf("%lld\n", lucas(a, b, p));
    }

    return 0;
}
```

## [AcWing **888. 求组合数 IV**](https://www.acwing.com/problem/content/890/)

输入 a, b，求 $C_a^b$ 的值。

注意结果可能很大，需要使用高精度计算。

### **输入格式**

共一行，包含两个整数 a 和 b。

### **输出格式**

共一行，输出  $C_a^b$ 的值。

### **数据范围**

1 ≤ b ≤ a ≤ 5000

### **输入样例：**

```
5 3
```

### **输出样例：**

```
10
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 5005;

int primes[N], cnt;
bool st[N];
int sum[N];

void get_primes(int n) {
    for (int i = 2; i <= n; i++) {
        if (!st[i]) primes[cnt++] = i;
        for (int j = 0; primes[j] * i <= n; j++) {
            st[primes[j] * i] = true;
            if (i % primes[j] == 0) break;
        }
    }
}

int get(int n, int p) {
    int res = 0;
    while (n) {
        res += n / p;
        n /= p;
    }
    return res;
}

vector<int> mul(vector<int> &a, int b) {
    vector<int> c;
    int t = 0;
    for (int i: a) {
        t += i * b;
        c.push_back(t % 10);
        t /= 10;
    }
    while (t) {
        c.push_back(t % 10);
        t /= 10;
    }
    return c;
}

int main() {
    int a, b;
    cin >> a >> b;
    get_primes(a);
    for (int i = 0; i < cnt; i++) {
        int p = primes[i];
        sum[i] = get(a, p) - get(a - b, p) - get(b, p);
    }
    vector<int> res;
    res.push_back(1);
    for (int i = 0; i < cnt; i++) {
        for (int j = 0; j < sum[i]; j++) {
            res = mul(res, primes[i]);
        }
    }
    for (int i = (int) res.size() - 1; i >= 0; i--) cout << res[i];
    cout << endl;
    return 0;
}
```