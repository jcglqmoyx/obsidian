---
page-title: "2858. 可以到达每一个节点的最少边反转次数 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/minimum-edge-reversals-so-every-node-is-reachable/description/
date: "2023-09-24 17:38:33"
---

> [2858\. Minimum Edge Reversals So Every Node Is Reachable](https://leetcode.cn/problems/minimum-edge-reversals-so-every-node-is-reachable/)

---

There is a **simple directed graph** with `n` nodes labeled from `0` to `n - 1`. The graph would form a **tree** if its edges were bi-directional.

You are given an integer `n` and a **2D** integer array `edges`, where `edges[i] = [ui, vi]` represents a **directed edge** going from node `ui` to node `vi`.

An **edge reversal** changes the direction of an edge, i.e., a directed edge going from node `ui` to node `vi` becomes a directed edge going from node `vi` to node `ui`.

For every node `i` in the range `[0, n - 1]`, your task is to **independently** calculate the **minimum** number of **edge reversals** required so it is possible to reach any other node starting from node `i` through a **sequence** of **directed edges**.

Return *an integer array* `answer`*, where* `answer[i]` *is the* ***minimum** number of **edge reversals** required so it is possible to reach any other node starting from node* `i` *through a **sequence** of **directed edges**.*

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/08/26/image-20230826221104-3.png)

**Input:** n = 4, edges = \[\[2,0\],\[2,1\],\[1,3\]\]
**Output:** \[1,1,0,2\]
**Explanation:** The image above shows the graph formed by the edges.
For node 0: after reversing the edge \[2,0\], it is possible to reach any other node starting from node 0.
So, answer\[0\] = 1.
For node 1: after reversing the edge \[2,1\], it is possible to reach any other node starting from node 1.
So, answer\[1\] = 1.
For node 2: it is already possible to reach any other node starting from node 2.
So, answer\[2\] = 0.
For node 3: after reversing the edges \[1,3\] and \[2,1\], it is possible to reach any other node starting from node 3.
So, answer\[3\] = 2.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/08/26/image-20230826225541-2.png)

**Input:** n = 3, edges = \[\[1,2\],\[2,0\]\]
**Output:** \[2,0,1\]
**Explanation:** The image above shows the graph formed by the edges.
For node 0: after reversing the edges \[2,0\] and \[1,2\], it is possible to reach any other node starting from node 0.
So, answer\[0\] = 2.
For node 1: it is already possible to reach any other node starting from node 1.
So, answer\[1\] = 0.
For node 2: after reversing the edge \[1, 2\], it is possible to reach any other node starting from node 2.
So, answer\[2\] = 1.

**Constraints:**

-   `2 <= n <= 105`
-   `edges.length == n - 1`
-   `edges[i].length == 2`
-   `0 <= ui == edges[i][0] < n`
-   `0 <= vi == edges[i][1] < n`
-   `ui != vi`
-   The input is generated such that if the edges were bi-directional, the graph would be a tree.
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    vector<int> minEdgeReversals(int n, vector<vector<int>> &edges) {  
        int h[n], e[n << 1], ne[n << 1], idx = 0;  
        memset(h, -1, sizeof h);  
        bool pass[n << 1];  
        int down[n], up[n];  
        memset(down, 0, sizeof down), memset(up, 0, sizeof up);  
        auto add = [&](int a, int b, bool flag) {  
            e[idx] = b, pass[idx] = flag, ne[idx] = h[a], h[a] = idx++;  
        };  
        for (auto &edge: edges) {  
            add(edge[0], edge[1], true);  
            add(edge[1], edge[0], false);  
        }  
        auto dfs1 = [&](auto &&dfs1, int u, int p) -> int {  
            for (int i = h[u]; ~i; i = ne[i]) {  
                int j = e[i];  
                if (j == p) continue;  
                int t = dfs1(dfs1, j, u);  
                down[u] += t + !pass[i];  
            }  
            return down[u];  
        };  
        dfs1(dfs1, 0, -1);  
        vector<int> res(n);  
        auto dfs2 = [&](auto &&dfs2, int u, int p, bool flag) -> void {  
            if (p != -1) {  
                up[u] = up[p] + (down[p] - down[u] - !flag) + flag;  
            }  
            res[u] = down[u] + up[u];  
            for (int i = h[u]; ~i; i = ne[i]) {  
                int j = e[i];  
                if (j == p) continue;  
                dfs2(dfs2, j, u, pass[i]);  
            }  
        };  
        dfs2(dfs2, 0, -1, false);  
        return res;  
    }  
}
```