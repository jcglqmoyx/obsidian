# 矩阵加速（数列）

## 题目描述

已知一个数列 $a$，它满足：  

$$
a_x=
\begin{cases}
 1 & x \in\{1,2,3\}\\ 
 a_{x-1}+a_{x-3} & x \geq 4
\end{cases}
$$

求 $a$ 数列的第 $n$ 项对 $10^9+7$ 取余的值。

## 输入格式

第一行一个整数 $T$，表示询问个数。

以下 $T$ 行，每行一个正整数 $n$。

## 输出格式

每行输出一个非负整数表示答案。

## 样例 #1

### 样例输入 #1

```
3
6
8
10
```

### 样例输出 #1

```
4
9
19
```

## 提示

- 对于 $30\%$ 的数据 $n \leq 100$；
- 对于 $60\%$ 的数据 $n \leq2 \times 10^7$；
- 对于 $100\%$ 的数据 $1 \leq T \leq 100$，$1 \leq n \leq 2 \times 10^9$。


```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
using LL = long long;  
  
const int MOD = 1e9 + 7;  
  
vector<vector<LL>> multiple(vector<vector<LL>> a, vector<vector<LL>> b) {  
    auto n = a.size(), m = b.size(), t = b[0].size();  
    vector<vector<LL>> res(n, vector<LL>(m));  
    for (int i = 0; i < n; i++) {  
        for (int j = 0; j < m; j++) {  
            for (int k = 0; k < t; k++) {  
                res[i][j] = (res[i][j] + a[i][k] * b[k][j]) % MOD;  
            }  
        }  
    }  
    return res;  
}  
  
vector<vector<LL>> pow(vector<vector<LL>> a, int k) {  
    auto res = a;  
    k--;  
    while (k) {  
        if (k & 1) res = multiple(res, a);  
        a = multiple(a, a);  
        k >>= 1;  
    }  
    return res;  
}  
  
void solve() {  
    int n;  
    scanf("%d", &n);  
    if (n <= 3) {  
        printf("%d\n", 1);  
    } else {  
        n -= 3;  
        vector<vector<LL>> a = {{1, 1, 0},  
                                {0, 0, 1},  
                                {1, 0, 0}};  
        vector<vector<LL>> f = {{1, 1, 1}};  
        auto matrix = multiple(f, pow(a, n));  
        printf("%lld\n", matrix[0][0]);  
    }  
}  
  
int main() {  
    int testcases;  
    scanf("%d", &testcases);  
    while (testcases--) solve();  
    return 0;  
}
```