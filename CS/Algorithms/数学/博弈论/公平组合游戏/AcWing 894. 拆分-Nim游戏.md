## [AcWing 894. 拆分-Nim游戏](https://www.acwing.com/problem/content/896/)

给定 n 堆石子，两位玩家轮流操作，每次操作可以取走其中的一堆石子，然后放入两堆**规模更小**的石子（新堆规模可以为 0，且两个新堆的石子总数可以大于取走的那堆石子数），最后无法进行操作的人视为失败。

问如果两人都采用最优策略，先手是否必胜。

### **输入格式**

第一行包含整数 n。

第二行包含 n 个整数，其中第 i 个整数表示第 i 堆石子的数量 $a_i$。

### **输出格式**

如果先手方必胜，则输出 `Yes`。

否则，输出 `No`。

### **数据范围**

1 ≤ n, $a_i$ ≤ 100

### **输入样例：**

```
2
2 3
```

### **输出样例：**

```
Yes
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 110;

int n;
int f[N];

int sg(int x) {
    if (f[x] != -1) return f[x];
    unordered_set<int> S;
    for (int i = 0; i < x; i++) {
        for (int j = 0; j <= i; j++) {
            S.insert(sg(i) ^ sg(j));
        }
    }
    for (int i = 0;; i++) {
        if (!S.count(i)) {
            return f[x] = i;
        }
    }
}

int main() {
    cin >> n;

    memset(f, -1, sizeof f);

    int res = 0;
    while (n--) {
        int x;
        cin >> x;
        res ^= sg(x);
    }

    if (res) puts("Yes");
    else puts("No");

    return 0;
}
```