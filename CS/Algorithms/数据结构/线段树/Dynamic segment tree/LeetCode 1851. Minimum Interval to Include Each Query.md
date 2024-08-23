---
page-title: LeetCode 1851. Minimum Interval to Include Each Query
url: https://leetcode.com/problems/minimum-interval-to-include-each-query/description/
date: 2024-08-23 15:20:54
---
You are given a 2D integer array `intervals`, where `intervals[i] = [lefti, righti]` describes the `ith` interval starting at `lefti` and ending at `righti` **(inclusive)**. The **size** of an interval is defined as the number of integers it contains, or more formally `righti - lefti + 1`.

You are also given an integer array `queries`. The answer to the `jth` query is the **size of the smallest interval** `i` such that `lefti <= queries[j] <= righti`. If no such interval exists, the answer is `-1`.

Return *an array containing the answers to the queries*.

**Example 1:**

**Input:** intervals = \[\[1,4\],\[2,4\],\[3,6\],\[4,4\]\], queries = \[2,3,4,5\]
**Output:** \[3,3,1,4\]
**Explanation:** The queries are processed as follows:
- Query = 2: The interval \[2,4\] is the smallest interval containing 2. The answer is 4 - 2 + 1 = 3.
- Query = 3: The interval \[2,4\] is the smallest interval containing 3. The answer is 4 - 2 + 1 = 3.
- Query = 4: The interval \[4,4\] is the smallest interval containing 4. The answer is 4 - 4 + 1 = 1.
- Query = 5: The interval \[3,6\] is the smallest interval containing 5. The answer is 6 - 3 + 1 = 4.

**Example 2:**

**Input:** intervals = \[\[2,3\],\[2,5\],\[1,8\],\[20,25\]\], queries = \[2,19,5,22\]
**Output:** \[2,-1,4,6\]
**Explanation:** The queries are processed as follows:
- Query = 2: The interval \[2,3\] is the smallest interval containing 2. The answer is 3 - 2 + 1 = 2.
- Query = 19: None of the intervals contain 19. The answer is -1.
- Query = 5: The interval \[2,5\] is the smallest interval containing 5. The answer is 5 - 2 + 1 = 4.
- Query = 22: The interval \[20,25\] is the smallest interval containing 22. The answer is 25 - 20 + 1 = 6.

**Constraints:**

-   `1 <= intervals.length <= 105`
-   `1 <= queries.length <= 105`
-   `intervals[i].length == 2`
-   `1 <= lefti <= righti <= 107`
-   `1 <= queries[j] <= 107`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    vector<int> minInterval(vector<vector<int>> &intervals, vector<int> &queries) {
        struct T {
            int l, r;

            bool operator<(const T &t) const {
                return r - l > t.r - t.l;
            }
        };
        priority_queue<T> q;
        for (auto &interval: intervals) {
            q.emplace(interval[0], interval[1]);
        }
        auto m = queries.size();
        set<pair<int, int>> s = {{-1, 0}, {1e9, 0}};
        for (int i = 0; i < m; i++) {
            s.emplace(queries[i], i);
        }
        vector<int> res(m, -1);
        while (!q.empty()) {
            auto t = q.top();
            q.pop();
            auto it = s.upper_bound({t.r, 1e9});
            it--;
            while ((*it).first != -1) {
                auto pre = prev(it);
                if ((*it).first >= t.l) {
                    res[(*it).second] = t.r - t.l + 1;
                    s.erase(it);
                } else {
                    break;
                }
                it = pre;
            }
        }

        return res;
    }
};
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 4e6;
const int INF = 0x3f3f3f3f;

int ls[N], rs[N], idx;
int L[N], R[N];
int mn[N];
bool lazy[N];

void extend(int u) {
    ls[u] = ++idx;
    rs[u] = ++idx;
    int mid = (L[u] + R[u]) >> 1;
    L[ls[u]] = L[u];
    R[ls[u]] = mid;
    L[rs[u]] = mid + 1;
    R[rs[u]] = R[u];
}

void push_down(int u) {
    if (lazy[u]) {
        lazy[ls[u]] = lazy[rs[u]] = true;
        mn[ls[u]] = min(mn[ls[u]], mn[u]);
        mn[rs[u]] = min(mn[rs[u]], mn[u]);
        lazy[u] = false;
    }
}

void update(int u, int l, int r) {
    if (!ls[u]) {
        extend(u);
    }
    if (L[u] >= l && R[u] <= r) {
        mn[u] = min(mn[u], r - l + 1);
        lazy[u] = true;
    } else {
        push_down(u);
        int mid = (L[u] + R[u]) >> 1;
        if (l <= mid) update(ls[u], l, r);
        if (r > mid) update(rs[u], l, r);
    }
}


int query(int u, int p) {
    if (!ls[u]) {
        extend(u);
    }
    if (L[u] == R[u]) return mn[u];
    push_down(u);
    int mid = (L[u] + R[u]) >> 1;
    if (p <= mid) return query(ls[u], p);
    return query(rs[u], p);
}

class Solution {
public:
    vector<int> minInterval(vector<vector<int>> &intervals, vector<int> &queries) {
        int mx = 0;
        memset(ls, 0, sizeof ls);
        memset(rs, 0, sizeof rs);
        memset(mn, 0x3f, sizeof mn);
        memset(lazy, 0, sizeof lazy);
        L[0] = 1, R[0] = 1e7, idx = 0;

        for (auto &v: intervals) {
            update(0, v[0], v[1]);
        }
        auto m = queries.size();
        vector<int> res(m, -1);
        for (int i = 0; i < m; i++) {
            int t = query(0, queries[i]);
            if (t != INF) res[i] = t;
        }
        return res;
    }
};
```