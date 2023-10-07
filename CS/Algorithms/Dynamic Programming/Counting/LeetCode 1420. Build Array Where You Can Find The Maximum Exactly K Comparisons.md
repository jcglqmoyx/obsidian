---
page-title: "1420. 生成数组 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/build-array-where-you-can-find-the-maximum-exactly-k-comparisons/
date: "2023-10-08 01:10:08"
---
You are given three integers `n`, `m` and `k`. Consider the following algorithm to find the maximum element of an array of positive integers:

![](https://assets.leetcode.com/uploads/2020/04/02/e.png)

You should build the array arr which has the following properties:

-   `arr` has exactly `n` integers.
-   `1 <= arr[i] <= m` where `(0 <= i < n)`.
-   After applying the mentioned algorithm to `arr`, the value `search_cost` is equal to `k`.

Return *the number of ways* to build the array `arr` under the mentioned conditions. As the answer may grow large, the answer **must be** computed modulo `109 + 7`.

**Example 1:**

**Input:** n = 2, m = 3, k = 1
**Output:** 6
**Explanation:** The possible arrays are \[1, 1\], \[2, 1\], \[2, 2\], \[3, 1\], \[3, 2\] \[3, 3\]

**Example 2:**

**Input:** n = 5, m = 2, k = 3
**Output:** 0
**Explanation:** There are no possible arrays that satisify the mentioned conditions.

**Example 3:**

**Input:** n = 9, m = 1, k = 1
**Output:** 1
**Explanation:** The only possible array is \[1, 1, 1, 1, 1, 1, 1, 1, 1\]

**Constraints:**

-   `1 <= n <= 50`
-   `1 <= m <= 100`
-   `0 <= k <= n`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int numOfArrays(int n, int m, int k) {
        if (!k || k > m) return 0;
        int mod = 1e9 + 7;
        int f[n + 1][m + 1][k + 1];
        memset(f, 0, sizeof f);
        for (int j = 1; j <= m; j++) f[1][j][1] = 1;
        for (int i = 2; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                for (int c = 1; c <= min(m, k); c++) {
                    int &v = f[i][j][c];
                    v = (long long) f[i - 1][j][c] * j % mod;
                    for (int u = 1; u < min(m + 1, j); u++) {
                        v = (v + f[i - 1][u][c - 1]) % mod;
                    }
                }
            }
        }
        int res = 0;
        for (int j = 1; j <= m; j++) {
            res = (res + f[n][j][k]) % mod;
        }
        return res;
    }
};
```