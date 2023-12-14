---
page-title: "Stamping the Grid - LeetCode"
url: https://leetcode.com/problems/stamping-the-grid/description/?envType=daily-question&envId=2023-12-14
date: "2023-12-14 11:32:43"
---
You are given an `m x n` binary matrix `grid` where each cell is either `0` (empty) or `1` (occupied).

You are then given stamps of size `stampHeight x stampWidth`. We want to fit the stamps such that they follow the given **restrictions** and **requirements**:

1.  Cover all the **empty** cells.
2.  Do not cover any of the **occupied** cells.
3.  We can put as **many** stamps as we want.
4.  Stamps can **overlap** with each other.
5.  Stamps are not allowed to be **rotated**.
6.  Stamps must stay completely **inside** the grid.

Return `true` *if it is possible to fit the stamps while following the given restrictions and requirements. Otherwise, return* `false`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/11/03/ex1.png)

**Input:** grid = \[\[1,0,0,0\],\[1,0,0,0\],\[1,0,0,0\],\[1,0,0,0\],\[1,0,0,0\]\], stampHeight = 4, stampWidth = 3
**Output:** true
**Explanation:** We have two overlapping stamps (labeled 1 and 2 in the image) that are able to cover all the empty cells.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/11/03/ex2.png)

**Input:** grid = \[\[1,0,0,0\],\[0,1,0,0\],\[0,0,1,0\],\[0,0,0,1\]\], stampHeight = 2, stampWidth = 2 
**Output:** false 
**Explanation:** There is no way to fit the stamps onto all the empty cells without the stamps going outside the grid.

**Constraints:**

-   `m == grid.length`
-   `n == grid[r].length`
-   `1 <= m, n <= 105`
-   `1 <= m * n <= 2 * 105`
-   `grid[r][c]` is either `0` or `1`.
-   `1 <= stampHeight, stampWidth <= 105`

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    bool possibleToStamp(vector<vector<int>>&grid, int stampHeight, int stampWidth) {  
        int m = grid.size(), n = grid[0].size();  
        vector<vector<int>> sum(m + 2, vector<int>(n + 2, 0));  
        vector<vector<int>> diff(m + 2, vector<int>(n + 2, 0));  
        for (int i = 1; i <= m; i++) {  
            for (int j = 1; j <= n; j++) {  
                sum[i][j] = sum[i - 1][j] + sum[i][j - 1] - sum[i - 1][j - 1] + grid[i - 1][j - 1];  
            }  
        }  
  
        for (int i = 1; i + stampHeight - 1 <= m; i++) {  
            for (int j = 1; j + stampWidth - 1 <= n; j++) {  
                int x = i + stampHeight - 1;  
                int y = j + stampWidth - 1;  
                if (sum[x][y] - sum[x][j - 1] - sum[i - 1][y] + sum[i - 1][j - 1] == 0) {  
                    diff[i][j]++;  
                    diff[i][y + 1]--;  
                    diff[x + 1][j]--;  
                    diff[x + 1][y + 1]++;  
                }  
            }  
        }  
  
        for (int i = 1; i <= m; i++) {  
            for (int j = 1; j <= n; j++) {  
                diff[i][j] += diff[i - 1][j] + diff[i][j - 1] - diff[i - 1][j - 1];  
                if (diff[i][j] == 0 && grid[i - 1][j - 1] == 0) {  
                    return false;  
                }  
            }  
        }  
        return true;  
    }  
};
```