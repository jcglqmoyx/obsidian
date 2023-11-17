---
page-title: "847. 访问所有节点的最短路径 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/shortest-path-visiting-all-nodes/description/?envType=daily-question&envId=2023-09-17
date: "2023-09-18 18:28:22"
---

> [847\. Shortest Path Visiting All Nodes](https://leetcode.cn/problems/shortest-path-visiting-all-nodes/)

---

You have an undirected, connected graph of `n` nodes labeled from `0` to `n - 1`. You are given an array `graph` where `graph[i]` is a list of all the nodes connected with node `i` by an edge.

Return *the length of the shortest path that visits every node*. You may start and stop at any node, you may revisit nodes multiple times, and you may reuse edges.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/12/shortest1-graph.jpg)

**Input:** graph = \[\[1,2,3\],\[0\],\[0\],\[0\]\]
**Output:** 4
**Explanation:** One possible path is \[1,0,2,0,3\]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/05/12/shortest2-graph.jpg)

**Input:** graph = \[\[1\],\[0,2,4\],\[1,3,4\],\[2\],\[1,2\]\]
**Output:** 4
**Explanation:** One possible path is \[0,1,4,2,3\]

**Constraints:**

-   `n == graph.length`
-   `1 <= n <= 12`
-   `0 <= graph[i].length < n`
-   `graph[i]` does not contain `i`.
-   If `graph[a]` contains `b`, then `graph[b]` contains `a`.
-   The input graph is always connected.
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int shortestPathLength(vector<vector<int>> &graph) {
        int n = (int) graph.size();
        int f[n][1 << n];
        memset(f, 0x3f, sizeof f);
        queue<pair<int, int>> q;
        for (int i = 0; i < n; i++) {
            f[i][1 << i] = 0;
            q.emplace(i, 1 << i);
        }
        while (!q.empty()) {
            auto t = q.front();
            q.pop();
            int p = t.first, state = t.second;
            for (int ne: graph[p]) {
                int new_state = state | 1 << ne;
                if (f[ne][new_state] > f[p][state] + 1) {
                    f[ne][new_state] = f[p][state] + 1;
                    q.emplace(ne, new_state);
                }
            }
        }
        int res = 0;
        for (int i = 0; i < n; i++) {
            res = max(res, f[i][(1 << n) - 1]);
        }
        return res;
    }
};
```