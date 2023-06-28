## [AcWing **8. 二维费用的背包问题**](https://www.acwing.com/problem/content/description/8/)

有 N 件物品和一个容量是 V 的背包，背包能承受的最大重量是 M。

每件物品只能用一次。体积是 $v_i$，重量是 mimi，价值是 $w_i$。

求解将哪些物品装入背包，可使物品总体积不超过背包容量，总重量不超过背包可承受的最大重量，且价值总和最大。输出最大价值。

### **输入格式**

第一行三个整数，N, V, M，用空格隔开，分别表示物品件数、背包容积和背包可承受的最大重量。

接下来有 N 行，每行三个整数 $v_i$，$m_i$，$w_i$，用空格隔开，分别表示第 i 件物品的体积、重量和价值。

### **输出格式**

输出一个整数，表示最大价值。

### **数据范围**

0 < N ≤ 1000

0 < V, M ≤ 100

0 < $v_i$, $m_i$ ≤ 100

0 < $w_i$ ≤ 1000

### **输入样例**

```
4 5 6
1 2 3
2 4 4
3 4 5
4 5 6
```

### **输出样例：**

```
8
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 105;

int n, V, M;
int f[N][N];

int main() {
    cin >> n >> V >> M;
    for (int i = 0; i < n; i++) {
        int v, w, x;
        cin >> v >> w >> x;
        for (int j = V; j >= v; j--) {
            for (int k = M; k >= w; k--) {
                f[j][k] = max(f[j][k], f[j - v][k - w] + x);
            }
        }
    }
    cout << f[V][M] << endl;
    return 0;
}
```

## [AcWing **1020. 潜水员**](https://www.acwing.com/problem/content/description/1022/)

潜水员为了潜水要使用特殊的装备。

他有一个带2种气体的气缸：一个为氧气，一个为氮气。

让潜水员下潜的深度需要各种数量的氧和氮。

潜水员有一定数量的气缸。

每个气缸都有重量和气体容量。

潜水员为了完成他的工作需要特定数量的氧和氮。

他完成工作所需气缸的总重的最低限度的是多少？

例如：潜水员有5个气缸。每行三个数字为：氧，氮的（升）量和气缸的重量：

```
3 36 120

10 25 129

5 50 250

1 45 130

4 20 119
```

如果潜水员需要5升的氧和60升的氮则总重最小为249（1，2或者4，5号气缸）。

你的任务就是计算潜水员为了完成他的工作需要的气缸的重量的最低值。

### **输入格式**

第一行有2个整数 m，n。它们表示氧，氮各自需要的量。

第二行为整数 k 表示气缸的个数。

此后的 kk行，每行包括 $a_i$，$b_i$，$c_i$，3个整数。这些各自是：第 i 个气缸里的氧和氮的容量及气缸重量。

### **输出格式**

仅一行包含一个整数，为潜水员完成工作所需的气缸的重量总和的最低值。

### **数据范围**

1 ≤ m ≤ 21,

1 ≤ n ≤ 79,

1 ≤ k ≤ 1000,

1 ≤ $a_i$ ≤ 21,

1 ≤ $b_i$ ≤ 79,

1 ≤ $c_i$ ≤ 800

### **输入样例：**

```
5 60
5
3 36 120
10 25 129
5 50 250
1 45 130
4 20 119
```

### **输出样例：**

```
249
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 22, M = 80;

int n, m, k;
int f[N][M];

int main() {
    memset(f, 0x3f, sizeof f);
    f[0][0] = 0;
    cin >> n >> m >> k;
    while (k--) {
        int v1, v2, w;
        cin >> v1 >> v2 >> w;
        for (int i = n; i >= 0; i--) {
            for (int j = m; j >= 0; j--) {
                f[i][j] = min(f[i][j], f[max(0, i - v1)][max(0, j - v2)] + w);
            }
        }
    }
    cout << f[n][m] << endl;
    return 0;
}
```






## [LeetCode 879. Profitable Schemes](https://leetcode.cn/problems/profitable-schemes/)
There is a group of `n` members, and a list of various crimes they could commit. The `ith` crime generates a `profit[i]` and requires `group[i]` members to participate in it. If a member participates in one crime, that member can't participate in another crime.

Let's call a **profitable scheme** any subset of these crimes that generates at least `minProfit` profit, and the total number of members participating in that subset of crimes is at most `n`.

Return the number of schemes that can be chosen. Since the answer may be very large, **return it modulo** `109 + 7`.

**Example 1:**

**Input:** n = 5, minProfit = 3, group = \[2,2\], profit = \[2,3\]
**Output:** 2
**Explanation:** To make a profit of at least 3, the group could either commit crimes 0 and 1, or just crime 1.
In total, there are 2 schemes.

**Example 2:**

**Input:** n = 10, minProfit = 5, group = \[2,3,5\], profit = \[6,7,8\]
**Output:** 7
**Explanation:** To make a profit of at least 5, the group could commit any crimes, as long as they commit one.
There are 7 possible schemes: (0), (1), (2), (0,1), (0,2), (1,2), and (0,1,2).

**Constraints:**

-   `1 <= n <= 100`
-   `0 <= minProfit <= 100`
-   `1 <= group.length <= 100`
-   `1 <= group[i] <= 100`
-   `profit.length == group.length`
-   `0 <= profit[i] <= 100`
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int profitableSchemes(int n, int minProfit, vector<int> &group, vector<int> &profit) {
        const int MOD = 1e9 + 7;
        vector<vector<int>> f(n + 1, vector<int>(minProfit + 1));
        for (int i = 0; i <= n; i++) f[i][0] = 1;
        for (int i = 0; i < group.size(); i++) {
            int g = group[i], p = profit[i];
            for (int j = n; j >= g; j--) {
                for (int k = minProfit; k >= 0; k--) {
                    f[j][k] = (f[j][k] + f[j - g][max(0, k - p)]) % MOD;
                }
            }
        }
        return f[n][minProfit];
    }
};
```