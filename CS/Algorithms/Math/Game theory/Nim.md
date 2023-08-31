## [AcWing **891. Nim游戏**](https://www.acwing.com/problem/content/description/893/)

给定 n 堆石子，两位玩家轮流操作，每次操作可以从任意一堆石子中拿走任意数量的石子（可以拿完，但不能不拿），最后无法进行操作的人视为失败。

问如果两人都采用最优策略，先手是否必胜。

### **输入格式**

第一行包含整数 n。

第二行包含 n 个数字，其中第 i 个数字表示第 i 堆石子的数量。

### **输出格式**

如果先手方必胜，则输出 `Yes`。

否则，输出 `No`。

### **数据范围**

1 ≤ n ≤ $10^5$，

1 ≤ 每堆石子数 ≤ $10^9$

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

int main() {
    int n;
    cin >> n;
    int s = 0;
    for (int i = 0; i < n; i++) {
        int x;
        cin >> x;
        s ^= x;
    }
    if (s) cout << "Yes" << endl;
    else cout << "No" << endl;
    return 0;
}
```
## [AcWing **892. 台阶-Nim游戏**](https://www.acwing.com/problem/content/894/)

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

## [AcWing **893. 集合-Nim游戏**](https://www.acwing.com/problem/content/895/)

给定 n 堆石子以及一个由 k 个不同正整数构成的数字集合 S。

现在有两位玩家轮流操作，每次操作可以从任意一堆石子中拿取石子，每次拿取的石子数量必须包含于集合 S，最后无法进行操作的人视为失败。

问如果两人都采用最优策略，先手是否必胜。

### **输入格式**

第一行包含整数 k，表示数字集合 S 中数字的个数。

第二行包含 k 个整数，其中第 i 个整数表示数字集合 S 中的第 i 个数 $s_i$。

第三行包含整数 n。

第四行包含 n 个整数，其中第 i 个整数表示第 i 堆石子的数量 $h_i$。

### **输出格式**

如果先手方必胜，则输出 `Yes`。

否则，输出 `No`。

### **数据范围**

1 ≤ n, k ≤ 100，

1 ≤ $s_i$, $h_i$≤ 10000

### **输入样例：**

```
2
2 5
3
2 4 7
```

### **输出样例：**

```
Yes
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 110, M = 10010;

int n, m;
int s[N], f[M];

int sg(int x) {
    if (f[x] != -1) return f[x];
    unordered_set<int> S;
    for (int i = 0; i < m; i++) {
        int sum = s[i];
        if (x >= sum) S.insert(sg(x - sum));
    }
    for (int i = 0;; i++) {
        if (!S.count(i)) {
            return f[x] = i;
        }
    }
}

int main() {
    cin >> m;
    for (int i = 0; i < m; i++) cin >> s[i];
    cin >> n;

    memset(f, -1, sizeof f);

    int res = 0;
    for (int i = 0; i < n; i++) {
        int x;
        cin >> x;
        res ^= sg(x);
    }

    if (res) puts("Yes");
    else puts("No");

    return 0;
}
```

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