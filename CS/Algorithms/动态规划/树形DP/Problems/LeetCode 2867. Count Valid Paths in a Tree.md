---
page-title: "100047. 统计树中的合法路径数目 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/count-valid-paths-in-a-tree/description/
date: "2023-09-25 01:25:24"
---
There is an undirected tree with `n` nodes labeled from `1` to `n`. You are given the integer `n` and a 2D integer array `edges` of length `n - 1`, where `edges[i] = [ui, vi]` indicates that there is an edge between nodes `ui` and `vi` in the tree.

Return *the **number of valid paths** in the tree*.

A path `(a, b)` is **valid** if there exists **exactly one** prime number among the node labels in the path from `a` to `b`.

**Note** that:

-   The path `(a, b)` is a sequence of **distinct** nodes starting with node `a` and ending with node `b` such that every two adjacent nodes in the sequence share an edge in the tree.
-   Path `(a, b)` and path `(b, a)` are considered the **same** and counted only **once**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/08/27/example1.png)

**Input:** n = 5, edges = \[\[1,2\],\[1,3\],\[2,4\],\[2,5\]\]
**Output:** 4
**Explanation:** The pairs with exactly one prime number on the path between them are: 
- (1, 2) since the path from 1 to 2 contains prime number 2. 
- (1, 3) since the path from 1 to 3 contains prime number 3.
- (1, 4) since the path from 1 to 4 contains prime number 2.
- (2, 4) since the path from 2 to 4 contains prime number 2.
It can be shown that there are only 4 valid paths.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/08/27/example2.png)

**Input:** n = 6, edges = \[\[1,2\],\[1,3\],\[2,4\],\[3,5\],\[3,6\]\]
**Output:** 6
**Explanation:** The pairs with exactly one prime number on the path between them are: 
- (1, 2) since the path from 1 to 2 contains prime number 2.
- (1, 3) since the path from 1 to 3 contains prime number 3.
- (1, 4) since the path from 1 to 4 contains prime number 2.
- (1, 6) since the path from 1 to 6 contains prime number 3.
- (2, 4) since the path from 2 to 4 contains prime number 2.
- (3, 6) since the path from 3 to 6 contains prime number 3.
It can be shown that there are only 6 valid paths.

**Constraints:**

-   `1 <= n <= 105`
-   `edges.length == n - 1`
-   `edges[i].length == 2`
-   `1 <= ui, vi <= n`
-   The input is generated such that `edges` represent a valid tree
```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 100010;

bool st[N];
int primes[N / 10], idx;

int init = []() {
    st[1] = true;
    for (int i = 2; i < N; i++) {
        if (!st[i]) primes[idx++] = i;
        for (int j = 0; primes[j] * i < N; j++) {
            st[primes[j] * i] = true;
            if (i % primes[j] == 0) break;
        }
    }
    return 0;
}();

class Solution {
    vector<int> sz, nodes;
    vector<vector<int>> g;

    void dfs(int u, int p) {
        nodes.push_back(u);
        for (int x: g[u]) {
            if (x != p && st[x]) {
                dfs(x, u);
            }
        }
    }

public:
    long long countPaths(int n, vector<vector<int>> &edges) {
        using ll = long long;
        sz.resize(n + 1, -1);
        g.resize(n + 1);
        for (auto &e: edges) {
            g[e[0]].emplace_back(e[1]);
            g[e[1]].emplace_back(e[0]);
        }
        ll res = 0;
        for (int i = 1; i <= n; i++) {
            if (!st[i]) {
                int s = 0;
                for (int x: g[i]) {
                    if (sz[x] == -1 && st[x]) {
                        nodes.clear();
                        dfs(x, i);
                        sz[x] = (int) nodes.size();
                        for (int node: nodes) {
                            sz[node] = sz[x];
                        }
                    }
                    if (!st[x]) continue;
                    res += sz[x] * s;
                    s += sz[x];
                    res += sz[x];
                }
            }
        }
        return res;
    }
};
```