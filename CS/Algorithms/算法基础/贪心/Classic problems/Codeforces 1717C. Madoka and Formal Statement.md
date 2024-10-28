Given an array of integer $a_1, a_2, \ldots, a_n$. In one operation you can make $a_i := a_i + 1$ if $i < n$ and $a_i \leq a_{i + 1}$, or $i = n$ and $a_i \leq a_1$.

You need to check whether the array $a_1, a_2, \ldots, a_n$ can become equal to the array $b_1, b_2, \ldots, b_n$ in some number of operations (possibly, zero). Two arrays $a$ and $b$ of length $n$ are called equal if $a_i = b_i$ for all integers $i$ from $1$ to $n$.


## 输入格式

The input consists of multiple test cases. The first line contains a single integer $t$ ($1 \le t \le 4 \cdot 10^4$) — the number of test cases. Description of the test cases follows.

The first line of each test case contains a single integer $n$ ($2 \le n \le 2 \cdot 10^5$) – the length of the array.

The second line of each test case contains $n$ integers $a_1, \ldots, a_n$ ($1 \le a_i \le 10^9$) – the elements of the array $a$.

The third line of each test case contains $n$ integers $b_1, \ldots, b_n$ ($1 \le b_i \le 10^9$) – the elements of the array $b$.

It is guaranteed that the sum of $n$ over all test cases does not exceed $2 \cdot 10^5$.

## 输出格式

For each test case, output "YES" if you can get the array $b$, otherwise output "NO".

You may print each letter in any case (for example, "YES", "Yes", "yes", "yEs" will all be recognized as positive answer).

## 样例 #1

### 样例输入 #1
```
5
3
1 2 5
1 2 5
2
2 2
1 3
4
3 4 1 2
6 4 2 5
3
2 4 1
4 5 3
5
1 2 3 4 5
6 5 6 7 6
```

### 样例输出 #1
```
YES
NO
NO
NO
YES
```
## 提示

In the first test case, the array $a$ is already equal to the array $b$.

In the second test case, we can't get the array $b$, because to do this we need to decrease $a_1$.

In the fifth test case, we can apply operations in order to the elements with indices $4, 3, 3, 2, 2, 2, 1, 1, 1, 1$, and then get the array $[5,5,5,5,5]$. After that, you can apply operations in order to elements with indices $5, 4, 4, 3, 1$ and already get an array $[6, 5, 6, 7, 6]$.


```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
void solve() {  
    int n;  
    scanf("%d", &n);  
    int a[n], b[n];  
    for (int i = 0; i < n; i++) {  
        scanf("%d", &a[i]);  
    }  
    for (int i = 0; i < n; i++) {  
        scanf("%d", &b[i]);  
    }  
    for (int i = 0; i < n; i++) {  
        int j = (i + 1) % n;  
        if (a[i] > b[i]) {  
            puts("NO");  
            return;  
        } else if (a[i] < b[i]) {  
            if (b[i] > b[j] + 1) {  
                puts("NO");  
                return;  
            }  
        }  
    }  
    puts("YES");  
}  
  
int main() {  
    int testcases;  
    scanf("%d", &testcases);  
    while (testcases--) solve();  
    return 0;  
}
```