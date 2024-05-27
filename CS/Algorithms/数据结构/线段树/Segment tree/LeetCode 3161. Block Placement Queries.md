---
page-title: 3161. Block Placement Queries
url: https://leetcode.com/problems/block-placement-queries/description/?utm_source=LCUS&utm_medium=ip_redirect&utm_campaign=transfer2china
date: 2024-05-26 15:42:29
---
There exists an infinite number line, with its origin at 0 and extending towards the **positive** x-axis.

You are given a 2D array `queries`, which contains two types of queries:

1.  For a query of type 1, `queries[i] = [1, x]`. Build an obstacle at distance `x` from the origin. It is guaranteed that there is **no** obstacle at distance `x` when the query is asked.
2.  For a query of type 2, `queries[i] = [2, x, sz]`. Check if it is possible to place a block of size `sz` *anywhere* in the range `[0, x]` on the line, such that the block **entirely** lies in the range `[0, x]`. A block **cannot** be placed if it intersects with any obstacle, but it may touch it. Note that you do **not** actually place the block. Queries are separate.

Return a boolean array `results`, where `results[i]` is `true` if you can place the block specified in the `ith` query of type 2, and `false` otherwise.

**Example 1:**

**Input:** queries = \[\[1,2\],\[2,3,3\],\[2,3,1\],\[2,2,2\]\]

**Output:** \[false,true,true\]

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/04/22/example0block.png)**

For query 0, place an obstacle at `x = 2`. A block of size at most 2 can be placed before `x = 3`.

**Example 2:**

**Input:** queries = \[\[1,7\],\[2,7,6\],\[1,2\],\[2,7,5\],\[2,7,6\]\]

**Output:** \[true,true,false\]

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/04/22/example1block.png)**

-   Place an obstacle at `x = 7` for query 0. A block of size at most 7 can be placed before `x = 7`.
-   Place an obstacle at `x = 2` for query 2. Now, a block of size at most 5 can be placed before `x = 7`, and a block of size at most 2 before `x = 2`.

**Constraints:**

-   `1 <= queries.length <= 15 * 104`
-   `2 <= queries[i].length <= 3`
-   `1 <= queries[i][0] <= 2`
-   `1 <= x, sz <= min(5 * 104, 3 * queries.length)`
-   The input is generated such that for queries of type 1, no obstacle exists at distance `x` when the query is asked.
-   The input is generated such that there is at least one query of type 2.

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 50010;

struct Node {
    int l, r;
    int mx;
} tr[N << 2];

void build(int u, int l, int r) {
    tr[u] = {l, r, 0};
    if (l != r) {
        int mid = (l + r) >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
    }
}

void update(int u, int p, int v) {
    if (tr[u].l == tr[u].r) tr[u].mx = v;
    else {
        int mid = (tr[u].l + tr[u].r) >> 1;
        if (p <= mid) update(u << 1, p, v);
        else update(u << 1 | 1, p, v);
        tr[u].mx = max(tr[u << 1].mx, tr[u << 1 | 1].mx);
    }
}

int query(int u, int l, int r) {
    if (tr[u].l >= l && tr[u].r <= r) return tr[u].mx;
    int mid = (tr[u].l + tr[u].r) >> 1;
    int res = 0;
    if (l <= mid) res = query(u << 1, l, r);
    if (r > mid) res = max(res, query(u << 1 | 1, l, r));
    return res;
}

class Solution {
public:
    vector<bool> getResults(vector<vector<int>> &queries) {
        int mx = 0;
        for (auto &q: queries) mx = max(mx, q[1]);
        mx++;
        build(1, 0, mx);
        set<int> s{0, mx};
        vector<bool> res;
        for (auto &q: queries) {
            int type = q[0], x = q[1];
            auto it = s.lower_bound(x);
            int l = *prev(it), r = *it;
            if (type == 1) {
                s.emplace(x);
                update(1, x, x - l), update(1, r, r - x);
            } else if (type == 2) {
                int sz = q[2];
                res.emplace_back(max(query(1, 1, l), x - l) >= sz);
            }
        }
        return res;
    }
};
```