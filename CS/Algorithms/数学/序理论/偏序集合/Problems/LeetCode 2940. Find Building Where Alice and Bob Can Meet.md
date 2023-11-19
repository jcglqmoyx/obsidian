---
page-title: "Find Building Where Alice and Bob Can Meet - LeetCode"
url: https://leetcode.com/problems/find-building-where-alice-and-bob-can-meet/
date: "2023-11-19 13:32:28"
---
You are given a **0-indexed** array `heights` of positive integers, where `heights[i]` represents the height of the `ith` building.

If a person is in building `i`, they can move to any other building `j` if and only if `i < j` and `heights[i] < heights[j]`.

You are also given another array `queries` where `queries[i] = [ai, bi]`. On the `ith` query, Alice is in building `ai` while Bob is in building `bi`.

Return *an array* `ans` *where* `ans[i]` *is **the index of the leftmost building** where Alice and Bob can meet on the* `ith` *query*. *If Alice and Bob cannot move to a common building on query* `i`, *set* `ans[i]` *to* `-1`.

**Example 1:**

**Input:** heights = \[6,4,8,5,2,7\], queries = \[\[0,1\],\[0,3\],\[2,4\],\[3,4\],\[2,2\]\]
**Output:** \[2,5,-1,5,2\]
**Explanation:** In the first query, Alice and Bob can move to building 2 since heights\[0\] < heights\[2\] and heights\[1\] < heights\[2\]. 
In the second query, Alice and Bob can move to building 5 since heights\[0\] < heights\[5\] and heights\[3\] < heights\[5\]. 
In the third query, Alice cannot meet Bob since Alice cannot move to any other building.
In the fourth query, Alice and Bob can move to building 5 since heights\[3\] < heights\[5\] and heights\[4\] < heights\[5\].
In the fifth query, Alice and Bob are already in the same building.  
For ans\[i\] != -1, It can be shown that ans\[i\] is the leftmost building where Alice and Bob can meet.
For ans\[i\] == -1, It can be shown that there is no building where Alice and Bob can meet.

**Example 2:**

**Input:** heights = \[5,3,8,2,6,1,4,6\], queries = \[\[0,7\],\[3,5\],\[5,2\],\[3,0\],\[1,6\]\]
**Output:** \[7,6,-1,4,6\]
**Explanation:** In the first query, Alice can directly move to Bob's building since heights\[0\] < heights\[7\].
In the second query, Alice and Bob can move to building 6 since heights\[3\] < heights\[6\] and heights\[5\] < heights\[6\].
In the third query, Alice cannot meet Bob since Bob cannot move to any other building.
In the fourth query, Alice and Bob can move to building 4 since heights\[3\] < heights\[4\] and heights\[0\] < heights\[4\].
In the fifth query, Alice can directly move to Bob's building since heights\[1\] < heights\[6\].
For ans\[i\] != -1, It can be shown that ans\[i\] is the leftmost building where Alice and Bob can meet.
For ans\[i\] == -1, It can be shown that there is no building where Alice and Bob can meet.

**Constraints:**

-   `1 <= heights.length <= 5 * 104`
-   `1 <= heights[i] <= 109`
-   `1 <= queries.length <= 5 * 104`
-   `queries[i] = [ai, bi]`
-   `0 <= ai, bi <= heights.length - 1`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    vector<int> leftmostBuildingQueries(vector<int> &heights, vector<vector<int>> &queries) {
        struct Node {
            int val, idx;
            int type;

            bool operator<(const Node &node) const {
                if (val == node.val) return idx > node.idx;
                return val > node.val;
            }
        };
        int n = (int) heights.size(), m = (int) queries.size();
        vector<Node> v(n);
        vector<int> res(m);
        for (int i = 0; i < n; i++) v[i] = {heights[i], n - i, i};
        for (int i = 0; i < queries.size(); i++) {
            int a = queries[i][0], b = queries[i][1];
            if (a > b) swap(a, b);
            if (a == b || heights[a] < heights[b]) res[i] = b;
            else v.push_back({heights[a], n - b, -i - 1});
        }
        sort(v.begin(), v.end());

        int sz = (int) v.size();
        int tr[sz + 1], INF = 0x3f3f3f3f;
        memset(tr, 0x3f, sizeof tr);
        auto lb = [](int x) { return x & -x; };
        auto update = [&](int x, int val) {
            for (; x <= sz; x += lb(x)) tr[x] = min(tr[x], val);
        };
        auto query = [&](int x) {
            int res = INF;
            for (; x; x -= lb(x)) res = min(res, tr[x]);
            return res == INF ? -1 : res;
        };
        for (auto &e: v) {
            if (e.type < 0) {
                res[-(e.type + 1)] = query(e.idx - 1);
            } else {
                update(e.idx, e.type);
            }
        }
        return res;
    }
};
```