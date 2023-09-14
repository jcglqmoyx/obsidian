---
page-title: "834. 树中距离之和 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/sum-of-distances-in-tree/description/
date: "2023-09-15 00:42:38"
---
There is an undirected connected tree with `n` nodes labeled from `0` to `n - 1` and `n - 1` edges.

You are given the integer `n` and the array `edges` where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree.

Return an array `answer` of length `n` where `answer[i]` is the sum of the distances between the `ith` node in the tree and all other nodes.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist1.jpg)

**Input:** n = 6, edges = \[\[0,1\],\[0,2\],\[2,3\],\[2,4\],\[2,5\]\]
**Output:** \[8,12,6,10,10,10\]
**Explanation:** The tree is shown above.
We can see that dist(0,1) + dist(0,2) + dist(0,3) + dist(0,4) + dist(0,5)
equals 1 + 1 + 2 + 2 + 2 = 8.
Hence, answer\[0\] = 8, and so on.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist2.jpg)

**Input:** n = 1, edges = \[\]
**Output:** \[0\]

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist3.jpg)

**Input:** n = 2, edges = \[\[1,0\]\]
**Output:** \[1,1\]

**Constraints:**

-   `1 <= n <= 3 * 104`
-   `edges.length == n - 1`
-   `edges[i].length == 2`
-   `0 <= ai, bi < n`
-   `ai != bi`
-   The given input represents a valid tree.

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 30010, M = N << 1;

int tot;
int h[N], e[M], ne[M], idx;
int down[N], up[N], cnt[N];

void add(int a, int b) {
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

int dfs1(int u, int p) {
    down[u] = 0;
    cnt[u] = 1;
    for (int i = h[u]; ~i; i = ne[i]) {
        int j = e[i];
        if (j == p) continue;
        int t = dfs1(j, u);
        cnt[u] += cnt[j];
        down[u] += t + cnt[j];
    }
    return down[u];
}

void dfs2(int u, int p) {
    if (p == -1) up[u] = 0;
    else up[u] = up[p] + tot + down[p] - down[u] - cnt[u] * 2;
    for (int i = h[u]; ~i; i = ne[i]) {
        int j = e[i];
        if (j == p) continue;
        dfs2(j, u);
    }
}

class Solution {
public:
    vector<int> sumOfDistancesInTree(int n, vector<vector<int>> &edges) {
        tot = n, memset(h, -1, sizeof h), idx = 0;
        for (auto &edge: edges) {
            int a = edge[0], b = edge[1];
            add(a, b), add(b, a);
        }
        dfs1(0, -1);
        dfs2(0, -1);
        vector<int> res(n);
        for (int i = 0; i < n; i++) {
            res[i] = up[i] + down[i];
        }
        return res;
    }
};
```