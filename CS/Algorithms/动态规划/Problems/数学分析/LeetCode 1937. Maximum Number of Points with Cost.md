---
page-title: 1937. Maximum Number of Points with Cost
url: https://leetcode.cn/problems/maximum-number-of-points-with-cost/description/?envType=daily-question&envId=2024-08-17
date: 2024-08-17 14:33:37
---
You are given an `m x n` integer matrix `points` (**0-indexed**). Starting with `0` points, you want to **maximize** the number of points you can get from the matrix.

To gain points, you must pick one cell in **each row**. Picking the cell at coordinates `(r, c)` will **add** `points[r][c]` to your score.

However, you will lose points if you pick a cell too far from the cell that you picked in the previous row. For every two adjacent rows `r` and `r + 1` (where `0 <= r < m - 1`), picking cells at coordinates `(r, c1)` and `(r + 1, c2)` will **subtract** `abs(c1 - c2)` from your score.

Return *the **maximum** number of points you can achieve*.

`abs(x)` is defined as:

-   `x` for `x >= 0`.
-   `-x` for `x < 0`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/12/screenshot-2021-07-12-at-13-40-26-diagram-drawio-diagrams-net.png)

**Input:** points = \[\[1,2,3\],\[1,5,1\],\[3,1,1\]\]
**Output:** 9
**Explanation:**
The blue cells denote the optimal cells to pick, which have coordinates (0, 2), (1, 1), and (2, 0).
You add 3 + 5 + 3 = 11 to your score.
However, you must subtract abs(2 - 1) + abs(1 - 0) = 2 from your score.
Your final score is 11 - 2 = 9.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/07/12/screenshot-2021-07-12-at-13-42-14-diagram-drawio-diagrams-net.png)

**Input:** points = \[\[1,5\],\[2,3\],\[4,2\]\]
**Output:** 11
**Explanation:**
The blue cells denote the optimal cells to pick, which have coordinates (0, 1), (1, 1), and (2, 0).
You add 5 + 3 + 4 = 12 to your score.
However, you must subtract abs(1 - 1) + abs(1 - 0) = 1 from your score.
Your final score is 12 - 1 = 11.

**Constraints:**

-   `m == points.length`
-   `n == points[r].length`
-   `1 <= m, n <= 105`
-   `1 <= m * n <= 105`
-   `0 <= points[r][c] <= 105`

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    long long maxPoints(vector<vector<int>> &points) {  
        int n = points.size(), m = points[0].size();  
        long long f[m];  
        for (int j = 0; j < m; j++) {  
            f[j] = points[0][j];  
        }  
        for (int i = 1; i < n; i++) {  
            long long l[m], r[m];  
            for (int j = 0; j < m; j++) {  
                l[j] = f[j] + j;  
                r[j] = f[j] - j;  
            }  
            long long mx = INT64_MIN;  
            for (int j = 0; j < m; j++) {  
                mx = max(mx, l[j]);  
                f[j] = mx - j;  
            }  
            mx = INT64_MIN;  
            for (int j = m - 1; j >= 0; j--) {  
                mx = max(mx, r[j]);  
                f[j] = max(f[j], mx + j) + points[i][j];  
            }  
        }  
        return *max_element(f, f + m);  
    }  
};
```