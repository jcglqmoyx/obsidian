---
page-title: "1504. 统计全 1 子矩形 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/count-submatrices-with-all-ones/description/
date: "2023-09-30 18:27:46"
---
Given an `m x n` binary matrix `mat`, *return the number of **submatrices** that have all ones*.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/10/27/ones1-grid.jpg)

**Input:** mat = \[\[1,0,1\],\[1,1,0\],\[1,1,0\]\]
**Output:** 13
**Explanation:** 
There are 6 rectangles of side 1x1.
There are 2 rectangles of side 1x2.
There are 3 rectangles of side 2x1.
There is 1 rectangle of side 2x2. 
There is 1 rectangle of side 3x1.
Total number of rectangles = 6 + 2 + 3 + 1 + 1 = 13.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/10/27/ones2-grid.jpg)

**Input:** mat = \[\[0,1,1,0\],\[0,1,1,1\],\[1,1,1,0\]\]
**Output:** 24
**Explanation:** 
There are 8 rectangles of side 1x1.
There are 5 rectangles of side 1x2.
There are 2 rectangles of side 1x3. 
There are 4 rectangles of side 2x1.
There are 2 rectangles of side 2x2. 
There are 2 rectangles of side 3x1. 
There is 1 rectangle of side 3x2. 
Total number of rectangles = 8 + 5 + 2 + 4 + 2 + 2 + 1 = 24.

**Constraints:**

-   `1 <= m, n <= 150`
-   `mat[i][j]` is either `0` or `1`.

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int numSubmat(vector<vector<int>> &mat) {
        int n = (int) mat.size(), m = (int) mat[0].size();
        int row[n][m];
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                if (j == 0) row[i][j] = mat[i][j];
                else if (!mat[i][j]) row[i][j] = 0;
                else row[i][j] = row[i][j - 1] + 1;
            }
        }
        int res = 0;
        for (int j = 0; j < m; ++j) {
            stack<pair<int, int>> q;
            int i = 0, sum = 0;
            while (i <= n - 1) {
                int height = 1;
                while (!q.empty() && q.top().first > row[i][j]) {
                    sum -= q.top().second * (q.top().first - row[i][j]);
                    height += q.top().second;
                    q.pop();
                }
                sum += row[i][j], res += sum;
                q.emplace(row[i][j], height);
                i++;
            }
        }
        return res;
    }
};
```

