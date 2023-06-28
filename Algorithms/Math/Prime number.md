## [AcWing **867. 分解质因数**](https://www.acwing.com/problem/content/description/869/)

给定 n 个正整数 $a_i$，将每个数分解质因数，并按照质因数从小到大的顺序输出每个质因数的底数和指数。

### **输入格式**

第一行包含整数 n。

接下来 n 行，每行包含一个正整数 $a_i$。

### **输出格式**

对于每个正整数 $a_i$，按照从小到大的顺序输出其分解质因数后，每个质因数的底数和指数，每个底数和指数占一行。

每个正整数的质因数全部输出完毕后，输出一个空行。

### **数据范围**

1 ≤ n ≤ 100，

2 ≤ $a_i$ ≤ 2 * $10^9$

### **输入样例：**

```
2
6
8
```

### **输出样例：**

```
2 1
3 1

2 3
```

```cpp
#include <bits/stdc++.h>

using namespace std;

void solve(int x) {
    for (int i = 2; i * i <= x; i++) {
        if (x % i == 0) {
            int p = 0;
            while (x % i == 0) {
                x /= i;
                p++;
            }
            printf("%d %d\n", i, p);
        }
    }
    if (x > 1) printf("%d %d\n", x, 1);
    puts("");
}

int main() {
    int T;
    cin >> T;
    while (T--) {
        int x;
        cin >> x;
        solve(x);
    }
    return 0;
}
```

## [AcWing **868. 筛质数**](https://www.acwing.com/problem/content/870/)

给定一个正整数 n，请你求出 1 ∼ n 中质数的个数。

### **输入格式**

共一行，包含整数 n。

### **输出格式**

共一行，包含一个整数，表示 1 ∼ n 中质数的个数。

### **数据范围**

1 ≤ n ≤ $10^6$

### **输入样例：**

```
8
```

### **输出样例：**

```
4
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1e6 + 5;

int n;
bool st[N];
int prime[N], cnt;

void solve() {
    for (int i = 2; i <= n; i++) {
        if (!st[i]) {
            prime[cnt++] = i;
        }
        for (int j = 0; prime[j] * i <= n; j++) {
            st[prime[j] * i] = true;
            if (i % prime[j] == 0) break;
        }
    }
}

int main() {
    cin >> n;
    solve();
    cout << cnt << endl;
    return 0;
}
```