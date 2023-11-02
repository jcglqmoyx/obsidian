---
page-title: "Domino and Tromino Tiling - LeetCode"
url: https://leetcode.com/problems/domino-and-tromino-tiling/description/
date: "2023-11-02 14:20:35"
---
You have two types of tiles: a `2 x 1` domino shape and a tromino shape. You may rotate these shapes.

![](https://assets.leetcode.com/uploads/2021/07/15/lc-domino.jpg)

Given an integer n, return *the number of ways to tile an* `2 x n` *board*. Since the answer may be very large, return it **modulo** `109 + 7`.

In a tiling, every square must be covered by a tile. Two tilings are different if and only if there are two 4-directionally adjacent cells on the board such that exactly one of the tilings has both squares occupied by a tile.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/15/lc-domino1.jpg)

**Input:** n = 3
**Output:** 5
**Explanation:** The five different ways are show above.

**Example 2:**

**Input:** n = 1
**Output:** 1

**Constraints:**

-   `1 <= n <= 1000`

DP, O(n)
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int numTilings(int n) {
        if (n == 1) return 1;
        long long f[n][3];
        int MOD = 1e9 + 7;
        memset(f, 0, sizeof f);
        f[0][0] = f[1][1] = f[1][2] = 1;
        f[1][0] = 2;
        for (int i = 2; i < n; i++) {
            f[i][0] = (f[i - 1][0] + f[i - 2][0] + f[i - 1][1] + f[i - 1][2]) % MOD;
            f[i][1] = (f[i - 2][0] + f[i - 1][2]) % MOD;
            f[i][2] = (f[i - 2][0] + f[i - 1][1]) % MOD;
        }
        return (int) f[n - 1][0];
    }
};
```

Fast exponentiation, O(log n)
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
    using LL = long long;
    const int MOD = 1e9 + 7;

    vector<vector<LL>> times(vector<vector<LL>> &a, vector<vector<LL>> &b) const {
        int n1 = (int) a.size(), m1 = (int) a.size(), m2 = (int) b[0].size();
        vector<vector<LL>> res(n1, vector<LL>(m2));
        for (int i = 0; i < n1; i++) {
            for (int j = 0; j < m2; j++) {
                for (int k = 0; k < m1; k++) {
                    res[i][j] = (res[i][j] + a[i][k] * b[k][j]) % MOD;
                }
            }
        }
        return res;
    }

    vector<vector<LL>> quick_power(vector<vector<LL>> a, int n) {
        vector<vector<LL>> res = a;
        n--;
        while (n) {
            if (n & 1) res = times(res, a);
            a = times(a, a);
            n >>= 1;
        }
        return res;
    }

public:
    int numTilings(int n) {
        if (n <= 2) return n;
        vector<vector<LL>> matrix = {{1, 0, 0, 1},
                                     {1, 0, 1, 0},
                                     {1, 1, 0, 0},
                                     {1, 1, 1, 0}};
        auto t = quick_power(matrix, n - 2);
        return (int) ((2 * t[0][0] + t[1][0] + t[2][0] + t[3][0]) % MOD);
    }
};
```