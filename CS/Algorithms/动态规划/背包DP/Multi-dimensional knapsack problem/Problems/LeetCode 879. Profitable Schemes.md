---
page-title: "879. 盈利计划 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/profitable-schemes/
date: "2023-10-24 12:39:05"
---
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

O(group.length * n * sum(profit))

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int profitableSchemes(int n, int minProfit, vector<int> &group, vector<int> &profit) {
        const int MOD = 1e9 + 7;
        int s = 0;
        for (int p: profit) {
            s += p;
        }
        if (s < minProfit) {
            return 0;
        }
        int res = 0;
        int f[n + 1][s + 1];
        memset(f, 0, sizeof f);
        f[0][0] = 1;
        for (int idx = 0; idx < group.size(); idx++) {
            int g[n + 1][s + 1];
            memcpy(g, f, sizeof g);
            for (int i = n; i >= group[idx]; i--) {
                for (int j = s; j >= profit[idx]; j--) {
                    g[i][j] = (g[i][j] + f[i - group[idx]][j - profit[idx]]) % MOD;
                }
            }
            memcpy(f, g, sizeof g);
        }
        for (int i = 0; i <= n; i++) {
            for (int j = minProfit; j <= s; j++) {
                res = (res + f[i][j]) % MOD;
            }
        }
        return res;
    }
};
```

优化：
O(group.length * n * minProfit)

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int profitableSchemes(int n, int minProfit, vector<int> &group, vector<int> &profit) {
        const int MOD = 1e9 + 7;
        int f[n + 1][minProfit + 1];
        memset(f, 0, sizeof f);
        f[0][0] = 1;
        for (int k = 0; k < group.size(); k++) {
            for (int i = n; i >= group[k]; i--) {
                for (int j = minProfit; j >= 0; j--) {
                    f[i][j] = (f[i][j] + f[i - group[k]][max(0, j - profit[k])]) % MOD;
                }
            }
        }
        int res = 0;
        for (int i = 0; i <= n; i++) {
            res = (res + f[i][minProfit]) % MOD;
        }
        return res;
    }
};
```