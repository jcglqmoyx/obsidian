## [AcWing 867. 分解质因数](https://www.acwing.com/problem/content/description/869/)

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