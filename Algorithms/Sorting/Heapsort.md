**Heapsort初始化时的down操作是O(N)的！**

## [AcWing 838 (Heapsort template)](https://www.acwing.com/problem/content/description/840/)

输入一个长度为 $n$ 的整数数列，从小到大输出前 $m$ 小的数。

### **输入格式**

第一行包含整数 $n$ 和 $m$。

第二行包含 $n$ 个整数，表示整数数列。

### **输出格式**

共一行，包含 $m$ 个整数，表示整数数列中前 $m$ 小的数。

### **数据范围**

1 ≤ $m$ ≤ $n$ ≤ $10^5$，

1 ≤ 数列中元素 ≤ $10^9$

### **输入样例：**

```
5 3
4 5 1 3 2
```

### **输出样例：**

```
1 2 3
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 100010;

int n, m;
int a[N];

void down(int u) {
    int t = u;
    if (u * 2 <= n && a[t] > a[u * 2]) t = u * 2;
    if (u * 2 + 1 <= n && a[t] > a[u * 2 + 1]) t = u * 2 + 1;
    if (t != u) {
        swap(a[u], a[t]);
        down(t);
    }
}

int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i++) scanf("%d", &a[i]);
		// 这个for循环是O(N)的！
    for (int i = n / 2; i; i--) down(i);
    while (m--) {
        printf("%d ", a[1]);
        a[1] = a[n];
        n--;
        down(1);
    }
    return 0;
}
```