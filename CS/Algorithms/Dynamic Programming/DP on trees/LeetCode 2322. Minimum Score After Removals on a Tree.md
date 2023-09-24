---
page-title: "2322. 从树中删除边的最小分数 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/minimum-score-after-removals-on-a-tree/description/
date: "2023-09-22 00:35:43"
---
There is an undirected connected tree with `n` nodes labeled from `0` to `n - 1` and `n - 1` edges.

You are given a **0-indexed** integer array `nums` of length `n` where `nums[i]` represents the value of the `ith` node. You are also given a 2D integer array `edges` of length `n - 1` where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree.

Remove two **distinct** edges of the tree to form three connected components. For a pair of removed edges, the following steps are defined:

1.  Get the XOR of all the values of the nodes for **each** of the three components respectively.
2.  The **difference** between the **largest** XOR value and the **smallest** XOR value is the **score** of the pair.

-   For example, say the three components have the node values: `[4,5,7]`, `[1,9]`, and `[3,3,3]`. The three XOR values are `4 ^ 5 ^ 7 = **6**`, `1 ^ 9 = **8**`, and `3 ^ 3 ^ 3 = **3**`. The largest XOR value is `8` and the smallest XOR value is `3`. The score is then `8 - 3 = 5`.

Return *the **minimum** score of any possible pair of edge removals on the given tree*.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/05/03/ex1drawio.png)

**Input:** nums = \[1,5,5,4,11\], edges = \[\[0,1\],\[1,2\],\[1,3\],\[3,4\]\]
**Output:** 9
**Explanation:** The diagram above shows a way to make a pair of removals.
- The 1st component has nodes \[1,3,4\] with values \[5,4,11\]. Its XOR value is 5 ^ 4 ^ 11 = 10.
- The 2nd component has node \[0\] with value \[1\]. Its XOR value is 1 = 1.
- The 3rd component has node \[2\] with value \[5\]. Its XOR value is 5 = 5.
The score is the difference between the largest and smallest XOR value which is 10 - 1 = 9.
It can be shown that no other pair of removals will obtain a smaller score than 9.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/05/03/ex2drawio.png)

**Input:** nums = \[5,5,2,4,4,2\], edges = \[\[0,1\],\[1,2\],\[5,2\],\[4,3\],\[1,3\]\]
**Output:** 0
**Explanation:** The diagram above shows a way to make a pair of removals.
- The 1st component has nodes \[3,4\] with values \[4,4\]. Its XOR value is 4 ^ 4 = 0.
- The 2nd component has nodes \[1,0\] with values \[5,5\]. Its XOR value is 5 ^ 5 = 0.
- The 3rd component has nodes \[2,5\] with values \[2,2\]. Its XOR value is 2 ^ 2 = 0.
The score is the difference between the largest and smallest XOR value which is 0 - 0 = 0.
We cannot obtain a smaller score than 0.

**Constraints:**

-   `n == nums.length`
-   `3 <= n <= 1000`
-   `1 <= nums[i] <= 108`
-   `edges.length == n - 1`
-   `edges[i].length == 2`
-   `0 <= ai, bi < n`
-   `ai != bi`
-   `edges` represents a valid tree.
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int minimumScore(vector<int> &nums, vector<vector<int>> &edges) {
        int n = (int) nums.size();
        int xor_sum[n], in[n], out[n];

        int h[n], e[n << 1], ne[n << 1], idx = 0;
        memset(h, -1, sizeof h);
        auto add = [&](int a, int b) {
            e[idx] = b, ne[idx] = h[a], h[a] = idx++;
        };

        for (auto &edge: edges) {
            add(edge[0], edge[1]);
            add(edge[1], edge[0]);
        }

        function<int(int, int, int &)> dfs = [&](int u, int p, int &time) -> int {
            xor_sum[u] = nums[u];
            in[u] = time++;
            for (int i = h[u]; ~i; i = ne[i]) {
                int j = e[i];
                if (j != p) {
                    xor_sum[u] ^= dfs(j, u, time);
                }
            }
            out[u] = time++;
            return xor_sum[u];
        };

        int time = 0;
        dfs(0, -1, time);

        int tot = 0;
        for (int x: nums) tot ^= x;
       
        int res = 1e9;
        for (int i = 0; i < n - 2; i++) {
            int tf, tu;
            int a = edges[i][0], b = edges[i][1];
            if (in[a] > in[b]) tf = xor_sum[a], tu = a;
            else tf = xor_sum[b], tu = b;
            for (int j = i + 1; j < n - 1; j++) {
                int f1 = tf, f2, f3, u = tu, v;
                
                int x = edges[j][0], y = edges[j][1];
                if (in[x] > in[y]) f2 = xor_sum[x], v = x;
                else f2 = xor_sum[y], v = y;

                if (in[u] < in[v] && out[u] > out[v]) f3 = tot ^ f1, f1 ^= f2;
                else if (in[u] > in[v] && out[u] < out[v]) f3 = tot ^ f2, f2 ^= f1;
                else f3 = tot ^ f1 ^ f2;

                res = min(res, max(f1, max(f2, f3)) - min(f1, min(f2, f3)));
            }
        }
        return res;
    }
};
```