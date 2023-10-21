## [AcWing 830. 单调栈](https://www.acwing.com/problem/content/832/)

给定一个长度为 $N$ 的整数数列，输出每个数左边第一个比它小的数，如果不存在则输出 −1。

### **输入格式**

第一行包含整数 $N$，表示数列长度。

第二行包含 $N$ 个整数，表示整数数列。

### **输出格式**

共一行，包含 $N$ 个整数，其中第 $i$ 个数表示第 $i$ 个数的左边第一个比它小的数，如果不存在则输出 −1。

### **数据范围**

$1$ ≤ $N$ ≤ $10^5$

1 ≤ 数列中元素 ≤ $10^9$

### **输入样例：**

```
5
3 4 2 7 5
```

### **输出样例：**

```diff
-1 3 -1 2 2
```

### Using STL:

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 100010;

int n;

int main() {
    scanf("%d", &n);
    stack<int> stk;
    while (n--) {
        int x;
        scanf("%d", &x);
        while (!stk.empty() && stk.top() >= x) stk.pop();
        if (stk.empty()) printf("-1 ");
        else printf("%d ", stk.top());
        stk.push(x);
    }
    puts("");
    return 0;
}
```

### Not using STL:

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 100010;

int n;
int stk[N], idx;

int main() {
    scanf("%d", &n);
    while (n--) {
        int x;
        scanf("%d", &x);
        while (idx && stk[idx - 1] >= x) idx--;
        if (idx == 0) printf("-1 ");
        else printf("%d ", stk[idx - 1]);
        stk[idx++] = x;
    }
    puts("");
    return 0;
}
```

---