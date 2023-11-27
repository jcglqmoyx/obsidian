---
page-title: "Largest Submatrix With Rearrangements - LeetCode"
url: https://leetcode.com/problems/largest-submatrix-with-rearrangements/?envType=daily-question&envId=2023-11-26
date: "2023-11-26 17:50:28"
---
You are given a binary matrix `matrix` of size `m x n`, and you are allowed to rearrange the **columns** of the `matrix` in any order.

Return *the area of the largest submatrix within* `matrix` *where **every** element of the submatrix is* `1` *after reordering the columns optimally.*

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/29/screenshot-2020-12-30-at-40536-pm.png)

**Input:** matrix = \[\[0,0,1\],\[1,1,1\],\[1,0,1\]\]
**Output:** 4
**Explanation:** You can rearrange the columns as shown above.
The largest submatrix of 1s, in bold, has an area of 4.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/29/screenshot-2020-12-30-at-40852-pm.png)

**Input:** matrix = \[\[1,0,1,0,1\]\]
**Output:** 3
**Explanation:** You can rearrange the columns as shown above.
The largest submatrix of 1s, in bold, has an area of 3.

**Example 3:**

**Input:** matrix = \[\[1,1,0\],\[1,0,1\]\]
**Output:** 2
**Explanation:** Notice that you must rearrange entire columns, and there is no way to make a submatrix of 1s larger than an area of 2.

**Constraints:**

-   `m == matrix.length`
-   `n == matrix[i].length`
-   `1 <= m * n <= 105`
-   `matrix[i][j]` is either `0` or `1`.

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int largestSubmatrix(vector<vector<int>>&matrix) {
        int res = 0, n = matrix.size(), m = matrix[0].size();
        int up[n][m];
        memset(up, 0, sizeof up);
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (matrix[i][j]) {
                    up[i][j] = i ? up[i - 1][j] + 1 : 1;
                } else {
                    up[i][j] = 0;
                }
            }
        }
        for (int i = 0; i < n; i++) {
            sort(up[i], up[i] + m, greater<>());
            for (int j = 0; j < m && up[i][j]; j++) {
                res = max(res, (j + 1) * up[i][j]);
            }
        }
        return res;
    }
};
```