---
page-title: LeetCode 3241. Time Taken to Mark All Nodes
url: https://leetcode.com/problems/time-taken-to-mark-all-nodes/description/
date: 2024-08-19 14:25:32
---
There exists an **undirected** tree with `n` nodes numbered `0` to `n - 1`. You are given a 2D integer array `edges` of length `n - 1`, where `edges[i] = [ui, vi]` indicates that there is an edge between nodes `ui` and `vi` in the tree.

Initially, **all** nodes are **unmarked**. For each node `i`:

-   If `i` is odd, the node will get marked at time `x` if there is **at least** one node *adjacent* to it which was marked at time `x - 1`.
-   If `i` is even, the node will get marked at time `x` if there is **at least** one node *adjacent* to it which was marked at time `x - 2`.

Return an array `times` where `times[i]` is the time when all nodes get marked in the tree, if you mark node `i` at time `t = 0`.

**Note** that the answer for each `times[i]` is **independent**, i.e. when you mark node `i` all other nodes are *unmarked*.

**Example 1:**

**Input:** edges = \[\[0,1\],\[0,2\]\]

**Output:** \[2,4,3\]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/06/01/screenshot-2024-06-02-122236.png)

-   For `i = 0`:
    -   Node 1 is marked at `t = 1`, and Node 2 at `t = 2`.
-   For `i = 1`:
    -   Node 0 is marked at `t = 2`, and Node 2 at `t = 4`.
-   For `i = 2`:
    -   Node 0 is marked at `t = 2`, and Node 1 at `t = 3`.

**Example 2:**

**Input:** edges = \[\[0,1\]\]

**Output:** \[1,2\]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/06/01/screenshot-2024-06-02-122249.png)

-   For `i = 0`:
    -   Node 1 is marked at `t = 1`.
-   For `i = 1`:
    -   Node 0 is marked at `t = 2`.

**Example 3:**

**Input:** edges = \[\[2,4\],\[0,1\],\[2,3\],\[0,2\]\]

**Output:** \[4,6,3,5,5\]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/06/03/screenshot-2024-06-03-210550.png)

**Constraints:**

-   `2 <= n <= 105`
-   `edges.length == n - 1`
-   `edges[i].length == 2`
-   `0 <= edges[i][0], edges[i][1] <= n - 1`
-   The input is generated such that `edges` represents a valid tree.

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    vector<int> timeTaken(vector<vector<int>> &edges) {
        auto n = edges.size() + 1;
        int h[n], e[n << 1], ne[n << 1], idx = 0;
        memset(h, -1, sizeof h);
        int down1[n], idx1[n], down2[n], up[n];
        memset(down1, 0, sizeof down1);
        memset(down2, 0, sizeof down2);
        memset(idx1, -1, sizeof idx1);
        memset(up, 0, sizeof up);

        auto add = [&](int a, int b) {
            e[idx] = b, ne[idx] = h[a], h[a] = idx++;
        };
        for (auto &edge: edges) {
            add(edge[0], edge[1]);
            add(edge[1], edge[0]);
        }
        auto dfs1 = [&](auto &&dfs1, int u, int p) -> int {
            for (int i = h[u]; ~i; i = ne[i]) {
                int j = e[i];
                if (j == p) continue;
                int t = dfs1(dfs1, j, u) + (1 + !(j & 1));
                if (t > down1[u]) {
                    down2[u] = down1[u];
                    down1[u] = t;
                    idx1[u] = j;
                } else if (t > down2[u]) {
                    down2[u] = t;
                }
            }
            return down1[u];
        };
        dfs1(dfs1, 0, -1);
        vector<int> res(n);
        auto dfs2 = [&](auto &&dfs2, int u, int p) -> void {
            if (p != -1) {
                up[u] = up[p];
                if (idx1[p] == u) up[u] = max(up[u], down2[p]);
                else up[u] = max(up[u], down1[p]);
                up[u] += 1 + !(p & 1);
            }
            res[u] = max(down1[u], up[u]);
            for (int i = h[u]; ~i; i = ne[i]) {
                int j = e[i];
                if (j == p) continue;
                dfs2(dfs2, j, u);
            }
        };
        dfs2(dfs2, 0, -1);
        return res;
    }
};
```