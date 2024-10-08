## [AcWing 892. 台阶-Nim游戏](https://www.acwing.com/problem/content/894/)

有一个 n 级台阶的楼梯，每级台阶上都有若干个石子，其中第 i 级台阶上有 $a_i$ 个石子(i ≥ 1)。

两位玩家轮流操作，每次操作可以从任意一级台阶上拿若干个石子放到下一级台阶中（不能不拿）。

已经拿到地面上的石子不能再拿，最后无法进行操作的人视为失败。

问如果两人都采用最优策略，先手是否必胜。

### **输入格式**

第一行包含整数 n。

第二行包含 n 个整数，其中第 i 个整数表示第 i 级台阶上的石子数 $a_i$。

### **输出格式**

如果先手方必胜，则输出 `Yes`。

否则，输出 `No`。

### **数据范围**

1 ≤ n ≤ $10^5$，

1 ≤ $a_i$ ≤ $10^9$

### **输入样例：**

```
3
2 1 3
```

### **输出样例：**

```
Yes
```

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
    int n;
    cin >> n;
    int s = 0;
    for (int i = 1; i <= n; i++) {
        int x;
        cin >> x;
        if (i & 1) s ^= x;
    }
    if (s) cout << "Yes" << endl;
    else cout << "No" << endl;
    return 0;
}
```