## [LeetCode 1595. Minimum Cost to Connect Two Groups of Points](https://leetcode.cn/problems/minimum-cost-to-connect-two-groups-of-points/)
---
You are given two groups of points where the first group has `size1` points, the second group has `size2` points, and `size1 >= size2`.

The `cost` of the connection between any two points are given in an `size1 x size2` matrix where `cost[i][j]` is the cost of connecting point `i` of the first group and point `j` of the second group. The groups are connected if **each point in both groups is connected to one or more points in the opposite group**. In other words, each point in the first group must be connected to at least one point in the second group, and each point in the second group must be connected to at least one point in the first group.

Return *the minimum cost it takes to connect the two groups*.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/03/ex1.jpg)

**Input:** cost = \[\[15, 96\], \[36, 2\]\]
**Output:** 17
**Explanation**: The optimal way of connecting the groups is:
1--A
2--B
This results in a total cost of 17.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/03/ex2.jpg)

**Input:** cost = \[\[1, 3, 5\], \[4, 1, 1\], \[1, 5, 3\]\]
**Output:** 4
**Explanation**: The optimal way of connecting the groups is:
1--A
2--B
2--C
3--A
This results in a total cost of 4.
Note that there are multiple points connected to point 2 in the first group and point A in the second group. This does not matter as there is no limit to the number of points that can be connected. We only care about the minimum total cost.

**Example 3:**

**Input:** cost = \[\[2, 5, 1\], \[3, 4, 7\], \[8, 1, 2\], \[6, 2, 4\], \[3, 8, 8\]\]
**Output:** 10

**Constraints:**

-   `size1 == cost.length`
-   `size2 == cost[i].length`
-   `1 <= size1, size2 <= 12`
-   `size1 >= size2`
-   `0 <= cost[i][j] <= 100`
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int connectTwoGroups(vector<vector<int>> &cost) {
        int n = (int) cost.size(), m = (int) cost[0].size();
        vector<vector<int>> prices(n, vector<int>(1 << m));
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < 1 << m; j++) {
                for (int k = 0; k < m; k++) {
                    if (j >> k & 1) {
                        prices[i][j] += cost[i][k];
                    }
                }
            }
        }
        vector<vector<int>> f(n, vector<int>(1 << m, 1e8));
        for (int j = 0; j < 1 << m; j++) {
            f[0][j] = prices[0][j];
        }
        for (int i = 1; i < n; i++) {
            f[i][0] = 0;
            for (int j = 0; j < 1 << m; j++) {
                for (int k = (j - 1) & j; k; k = (k - 1) & j) {
                    f[i][j] = min(f[i][j], f[i - 1][j ^ k] + prices[i][k]);
                }
                for (int k = 0; k < m; k++) {
                    if (j >> k & 1) {
                        f[i][j] = min(f[i][j], f[i - 1][j] + cost[i][k]);
                    }
                }
            }
        }
        return f[n - 1][(1 << m) - 1];
    }
};
```