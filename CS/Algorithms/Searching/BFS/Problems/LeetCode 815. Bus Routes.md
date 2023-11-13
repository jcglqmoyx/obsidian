---
page-title: "815. 公交路线 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/bus-routes/description/?envType=daily-question&envId=2023-11-12
date: "2023-11-12 18:00:50"
---
You are given an array `routes` representing bus routes where `routes[i]` is a bus route that the `ith` bus repeats forever.

-   For example, if `routes[0] = [1, 5, 7]`, this means that the `0th` bus travels in the sequence `1 -> 5 -> 7 -> 1 -> 5 -> 7 -> 1 -> ...` forever.

You will start at the bus stop `source` (You are not on any bus initially), and you want to go to the bus stop `target`. You can travel between bus stops by buses only.

Return *the least number of buses you must take to travel from* `source` *to* `target`. Return `-1` if it is not possible.

**Example 1:**

**Input:** routes = \[\[1,2,7\],\[3,6,7\]\], source = 1, target = 6
**Output:** 2
**Explanation:** The best strategy is take the first bus to the bus stop 7, then take the second bus to the bus stop 6.

**Example 2:**

**Input:** routes = \[\[7,12\],\[4,5,15\],\[6\],\[15,19\],\[9,12,13\]\], source = 15, target = 12
**Output:** -1

**Constraints:**

-   `1 <= routes.length <= 500`.
-   `1 <= routes[i].length <= 105`
-   All the values of `routes[i]` are **unique**.
-   `sum(routes[i].length) <= 105`
-   `0 <= routes[i][j] < 106`
-   `0 <= source, target < 106`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int numBusesToDestination(vector<vector<int>> &routes, int source, int target) {
        if (source == target) return 0;
        int n = (int) routes.size();
        unordered_map<int, vector<int>> map;
        vector<vector<int>> g(n);
        for (int i = 0; i < n; i++) {
            for (int x: routes[i]) {
                map[x].emplace_back(i);
            }
        }
        for (auto &[_, v]: map) {
            int m = (int) v.size();
            for (int i = 0; i < m; i++) {
                for (int j = i + 1; j < m; j++) {
                    g[v[i]].emplace_back(v[j]);
                    g[v[j]].emplace_back(v[i]);
                }
            }
        }
        bool S[n];
        memset(S, 0, sizeof S);
        for (int x: map[target]) {
            S[x] = true;
        }
        bool st[n];
        memset(st, 0, sizeof st);
        queue<int> q;
        for (int idx: map[source]) {
            if (S[idx]) return 1;
            q.emplace(idx);
            st[idx] = true;
        }
        int dist = 1;
        while (!q.empty()) {
            dist++;
            for (auto it = q.size(); it; it--) {
                int t = q.front();
                q.pop();
                for (int x: g[t]) {
                    if (!st[x]) {
                        if (S[x]) return dist;
                        q.emplace(x);
                        st[x] = true;
                    }
                }
            }
        }
        return -1;
    }
};
```