## [LeetCode 1210. Minimum Moves to Reach Target with Rotations](https://leetcode.cn/problems/minimum-moves-to-reach-target-with-rotations/description/)

In an `n*n` grid, there is a snake that spans 2 cells and starts moving from the top left corner at `(0, 0)` and `(0, 1)`. The grid has empty cells represented by zeros and blocked cells represented by ones. The snake wants to reach the lower right corner at `(n-1, n-2)` and `(n-1, n-1)`.

In one move the snake can:

- Move one cell to the right if there are no blocked cells there. This move keeps the horizontal/vertical position of the snake as it is.
- Move down one cell if there are no blocked cells there. This move keeps the horizontal/vertical position of the snake as it is.
- Rotate clockwise if it's in a horizontal position and the two cells under it are both empty. In that case the snake moves from `(r, c)` and `(r, c+1)` to `(r, c)` and `(r+1, c)`.
    
    ![https://assets.leetcode.com/uploads/2019/09/24/image-2.png](https://assets.leetcode.com/uploads/2019/09/24/image-2.png)
    
- Rotate counterclockwise if it's in a vertical position and the two cells to its right are both empty. In that case the snake moves from `(r, c)` and `(r+1, c)` to `(r, c)` and `(r, c+1)`.
    
    ![https://assets.leetcode.com/uploads/2019/09/24/image-1.png](https://assets.leetcode.com/uploads/2019/09/24/image-1.png)
    

Return the minimum number of moves to reach the target.

If there is no way to reach the target, return `-1`.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/09/24/image.png](https://assets.leetcode.com/uploads/2019/09/24/image.png)

```
Input: grid = [[0,0,0,0,0,1],
               [1,1,0,0,1,0],
               [0,0,0,0,1,1],
               [0,0,1,0,1,0],
               [0,1,1,0,0,0],
               [0,1,1,0,0,0]]
Output: 11
Explanation:
One possible solution is [right, right, rotate clockwise, right, down, down, down, down, rotate counterclockwise, right, down].
```

**Example 2:**

```
Input: grid = [[0,0,1,1,1,1],
               [0,0,0,0,1,1],
               [1,1,0,0,0,1],
               [1,1,1,0,0,1],
               [1,1,1,0,0,1],
               [1,1,1,0,0,0]]
Output: 9
```

**Constraints:**

- `2 <= n <= 100`
- `0 <= grid[i][j] <= 1`
- It is guaranteed that the snake starts at empty cells.

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int minimumMoves(vector<vector<int>> &grid) {
        int n = (int) grid.size(), m = (int) grid[0].size();
        bool st[n][m][2];
        memset(st, 0, sizeof st);
        st[0][0][0] = true;
        queue<tuple<int, int, int>> q;
        q.push({0, 0, 0});
        int step = 0;
        while (!q.empty()) {
            step++;
            for (auto it = q.size(); it; it--) {
                auto[x, y, d] = q.front();
                q.pop();
                if (d == 0) {
                    if (y + 2 < m && !grid[x][y + 2] && !st[x][y + 1][0]) {
                        if (x == n - 1 && y == m - 3) return step;
                        q.push({x, y + 1, 0});
                        st[x][y + 1][0] = true;
                    }
                    if (x + 1 < n && !grid[x + 1][y] && !grid[x + 1][y + 1] && !st[x + 1][y][0]) {
                        if (x == n - 2 && y == m - 2) return step;
                        q.push({x + 1, y, 0});
                        st[x + 1][y][0] = true;
                    }
                    if (x < n - 1 && !grid[x + 1][y] && !grid[x + 1][y + 1] && !st[x][y][1]) {
                        q.push({x, y, 1});
                        st[x][y][1] = true;
                    }
                } else {
                    if (x + 2 < m && !grid[x + 2][y] && !st[x + 1][y][1]) {
                        q.push({x + 1, y, 1});
                        st[x + 1][y][1] = true;
                    }
                    if (y + 1 < m && !grid[x][y + 1] && !grid[x + 1][y + 1] && !st[x][y + 1][1]) {
                        q.push({x, y + 1, 1});
                        st[x][y + 1][1] = true;
                    }
                    if (y + 1 < m && !grid[x][y + 1] && !grid[x + 1][y + 1] && !st[x][y][0]) {
                        q.push({x, y, 0});
                        st[x][y][0] = true;
                    }
                }
            }
        }
        return -1;
    }
};
```