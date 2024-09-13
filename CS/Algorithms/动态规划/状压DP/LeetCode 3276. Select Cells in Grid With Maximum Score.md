---
page-title: LeetCode 3276. Select Cells in Grid With Maximum Score
url: https://leetcode.cn/problems/select-cells-in-grid-with-maximum-score/description/
date: 2024-09-12 23:39:43
---
You are given a 2D matrix `grid` consisting of positive integers.

You have to select *one or more* cells from the matrix such that the following conditions are satisfied:

-   No two selected cells are in the **same** row of the matrix.
-   The values in the set of selected cells are **unique**.

Your score will be the **sum** of the values of the selected cells.

Return the **maximum** score you can achieve.

**Example 1:**

**Input:** grid = \[\[1,2,3\],\[4,3,2\],\[1,1,1\]\]

**Output:** 8

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/07/29/grid1drawio.png)

We can select the cells with values 1, 3, and 4 that are colored above.

**Example 2:**

**Input:** grid = \[\[8,7,6\],\[8,3,2\]\]

**Output:** 15

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/07/29/grid8_8drawio.png)

We can select the cells with values 7 and 8 that are colored above.

**Constraints:**

-   `1 <= grid.length, grid[i].length <= 10`
-   `1 <= grid[i][j] <= 100`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int maxScore(vector<vector<int>> &grid) {
        auto n = grid.size();
        int mx = 0;
        for (auto &r: grid) {
            mx = max(mx, *max_element(r.begin(), r.end()));
        }
        vector<vector<int>> pos(mx + 1);
        for (int i = 0; i < n; i++) {
            for (int x: grid[i]) {
                pos[x].emplace_back(i);
            }
        }
        for (auto &r: pos) {
            sort(r.begin(), r.end());
            r.erase(unique(r.begin(), r.end()), r.end());
        }
        int f[mx + 1][1 << n];
        memset(f, -1, sizeof f);

        auto dp = [&](auto &&dp, int i, int j) -> int {
            if (i == 0 || j == 0) return 0;
            if (f[i][j] != -1) return f[i][j];
            f[i][j] = dp(dp, i - 1, j);
            if (!pos[i].empty()) {
                for (int r: pos[i]) {
                    if (j >> r & 1) {
                        f[i][j] = max(f[i][j], i + dp(dp, i - 1, j ^ (1 << r)));
                    }
                }
            }
            return f[i][j];
        };
        return dp(dp, mx, (1 << n) - 1);
    }
};
```