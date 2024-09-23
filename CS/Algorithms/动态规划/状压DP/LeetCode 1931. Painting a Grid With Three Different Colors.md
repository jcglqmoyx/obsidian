---
page-title: LeetCode 1931. Painting a Grid With Three Different Colors
url: https://leetcode.com/problems/painting-a-grid-with-three-different-colors/description/
date: 2024-09-23 15:36:06
---
You are given two integers `m` and `n`. Consider an `m x n` grid where each cell is initially white. You can paint each cell **red**, **green**, or **blue**. All cells **must** be painted.

Return *the number of ways to color the grid with **no two adjacent cells having the same color***. Since the answer can be very large, return it **modulo** `109 + 7`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/22/colorthegrid.png)

**Input:** m = 1, n = 1
**Output:** 3
**Explanation:** The three possible colorings are shown in the image above.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/06/22/copy-of-colorthegrid.png)

**Input:** m = 1, n = 2
**Output:** 6
**Explanation:** The six possible colorings are shown in the image above.

**Example 3:**

**Input:** m = 5, n = 5
**Output:** 580986

**Constraints:**

-   `1 <= m <= 5`
-   `1 <= n <= 1000`


```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int colorTheGrid(int m, int n) {  
        int tot = 1, MOD = 1e9 + 7;  
        for (int i = 0; i < m; i++) tot *= 3;  
        int f[tot];  
        memset(f, 0, sizeof f);  
        vector<int> v[tot];  
        for (int i = 0; i < tot; i++) {  
            int mask = i;  
            for (int prev = -1, j = 0; j < m; j++) {  
                int t = mask % 3;  
                if (t == prev) {  
                    v[i].clear();  
                    break;  
                }  
                v[i].emplace_back(t);  
                mask /= 3;  
                prev = t;  
            }  
            if (!v[i].empty()) f[i] = 1;  
        }  
        for (int i = 1; i < n; i++) {  
            int g[tot];  
            memset(g, 0, sizeof g);  
            for (int cur = 0; cur < tot; cur++) {  
                if (v[cur].empty()) continue;  
                for (int last = 0; last < tot; last++) {  
                    if (v[last].empty()) continue;  
                    bool ok = true;  
                    for (int j = 0; j < m; j++) {  
                        if (v[cur][j] == v[last][j]) {  
                            ok = false;  
                            break;  
                        }  
                    }  
                    if (ok) g[cur] = (g[cur] + f[last]) % MOD;  
                }  
            }  
            memcpy(f, g, sizeof g);  
        }  
        int res = 0;  
        for (int i = 0; i < tot; i++) {  
            res = (res + f[i]) % MOD;  
        }  
        return res;  
    }  
};
```
