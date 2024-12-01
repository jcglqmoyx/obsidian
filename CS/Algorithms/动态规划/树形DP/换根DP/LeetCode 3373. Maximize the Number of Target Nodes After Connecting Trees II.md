---
page-title: LeetCode 3373. Maximize the Number of Target Nodes After Connecting Trees II
url: https://leetcode.com/problems/maximize-the-number-of-target-nodes-after-connecting-trees-ii/description/
date: 2024-12-01 12:52:26
---
There exist two **undirected** trees with `n` and `m` nodes, labeled from `[0, n - 1]` and `[0, m - 1]`, respectively.

You are given two 2D integer arrays `edges1` and `edges2` of lengths `n - 1` and `m - 1`, respectively, where `edges1[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the first tree and `edges2[i] = [ui, vi]` indicates that there is an edge between nodes `ui` and `vi` in the second tree.

Node `u` is **target** to node `v` if the number of edges on the path from `u` to `v` is even. **Note** that a node is *always* **target** to itself.

Return an array of `n` integers `answer`, where `answer[i]` is the **maximum** possible number of nodes that are **target** to node `i` of the first tree if you had to connect one node from the first tree to another node in the second tree.

**Note** that queries are independent from each other. That is, for every query you will remove the added edge before proceeding to the next query.

**Example 1:**

**Input:** edges1 = \[\[0,1\],\[0,2\],\[2,3\],\[2,4\]\], edges2 = \[\[0,1\],\[0,2\],\[0,3\],\[2,7\],\[1,4\],\[4,5\],\[4,6\]\]

**Output:** \[8,7,7,8,8\]

**Explanation:**

-   For `i = 0`, connect node 0 from the first tree to node 0 from the second tree.
-   For `i = 1`, connect node 1 from the first tree to node 4 from the second tree.
-   For `i = 2`, connect node 2 from the first tree to node 7 from the second tree.
-   For `i = 3`, connect node 3 from the first tree to node 0 from the second tree.
-   For `i = 4`, connect node 4 from the first tree to node 4 from the second tree.

![](https://assets.leetcode.com/uploads/2024/09/24/3982-1.png)

**Example 2:**

**Input:** edges1 = \[\[0,1\],\[0,2\],\[0,3\],\[0,4\]\], edges2 = \[\[0,1\],\[1,2\],\[2,3\]\]

**Output:** \[3,6,6,6,6\]

**Explanation:**

For every `i`, connect node `i` of the first tree with any node of the second tree.

![](https://assets.leetcode.com/uploads/2024/09/24/3928-2.png)

**Constraints:**

-   `2 <= n, m <= 105`
-   `edges1.length == n - 1`
-   `edges2.length == m - 1`
-   `edges1[i].length == edges2[i].length == 2`
-   `edges1[i] = [ai, bi]`
-   `0 <= ai, bi < n`
-   `edges2[i] = [ui, vi]`
-   `0 <= ui, vi < m`
-   The input is generated such that `edges1` and `edges2` represent valid trees.

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
pair<int, int> dfs1(vector<vector<int>> &g, vector<int> &d_o, vector<int> &de, int u, int p) {  
    pair<int, int> res = {0, 1};  
    for (int x: g[u]) {  
        if (x != p) {  
            auto t = dfs1(g, d_o, de, x, u);  
            res.first += t.second;  
            res.second += t.first;  
        }  
    }  
    d_o[u] = res.first;  
    de[u] = res.second;  
    return res;  
}  
  
void dfs2(vector<vector<int>> &g, vector<int> &d_o, vector<int> &de, vector<int> &uo, vector<int> &ue, int u, int p) {  
    if (p != -1) {  
        uo[u] = de[p] - d_o[u] + ue[p];  
        ue[u] = d_o[p] - de[u] + uo[p];  
    }  
    for (int x: g[u]) {  
        if (x != p) {  
            dfs2(g, d_o, de, uo, ue, x, u);  
        }  
    }  
};  
  
class Solution {  
public:  
    vector<int> maxTargetNodes(vector<vector<int>> &edges1, vector<vector<int>> &edges2) {  
        int n = static_cast<int>(edges1.size()) + 1, m = static_cast<int>(edges2.size()) + 1;  
        vector<vector<int>> g1(n), g2(m);  
        for (auto &e: edges1) {  
            g1[e[0]].emplace_back(e[1]);  
            g1[e[1]].emplace_back(e[0]);  
        }  
        for (auto &e: edges2) {  
            g2[e[0]].emplace_back(e[1]);  
            g2[e[1]].emplace_back(e[0]);  
        }  
        vector<int> do1(n), de1(n);  
        vector<int> do2(m), de2(m);  
        dfs1(g1, do1, de1, 0, -1);  
        dfs1(g2, do2, de2, 0, -1);  
  
        vector<int> uo1(n), ue1(n);  
        vector<int> uo2(m), ue2(m);  
        dfs2(g1, do1, de1, uo1, ue1, 0, -1);  
        dfs2(g2, do2, de2, uo2, ue2, 0, -1);  
        int mo2 = 0, me2 = 0;  
        for (int j = 0; j < m; j++) {  
            mo2 = max(mo2, do2[j] + uo2[j]);  
            me2 = max(me2, de2[j] + ue2[j]);  
        }  
        vector<int> res(n);  
        for (int i = 0; i < n; i++) {  
            int e = de1[i] + ue1[i];  
            res[i] = e + mo2;  
        }  
        return res;  
    }  
};
```