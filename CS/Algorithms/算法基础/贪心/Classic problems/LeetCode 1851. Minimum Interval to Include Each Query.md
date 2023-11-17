## [LeetCode 1851. Minimum Interval to Include Each Query](https://leetcode.cn/problems/minimum-interval-to-include-each-query/description/)

You are given a 2D integer array `intervals`, where `intervals[i] = [$left_i$, $right_i$]` describes the$`i^{th}`$ interval starting at$`left_i`$ and ending at$`right_i`$ **(inclusive)**. The **size** of an interval is defined as the number of integers it contains, or more formally$`right_i$ - $left_i$ + 1`.

You are also given an integer array `queries`. The answer to the$`j^{th}`$ query is the **size of the smallest interval** `i` such that$`left_i$ <= queries[j] <= $right_i$`. If no such interval exists, the answer is `-1`.

Return _an array containing the answers to the queries_.

**Example 1:**

```
Input: intervals = [[1,4],[2,4],[3,6],[4,4]], queries = [2,3,4,5]
Output: [3,3,1,4]
Explanation: The queries are processed as follows:
- Query = 2: The interval [2,4] is the smallest interval containing 2. The answer is 4 - 2 + 1 = 3.
- Query = 3: The interval [2,4] is the smallest interval containing 3. The answer is 4 - 2 + 1 = 3.
- Query = 4: The interval [4,4] is the smallest interval containing 4. The answer is 4 - 4 + 1 = 1.
- Query = 5: The interval [3,6] is the smallest interval containing 5. The answer is 6 - 3 + 1 = 4.
```

**Example 2:**

```
Input: intervals = [[2,3],[2,5],[1,8],[20,25]], queries = [2,19,5,22]
Output: [2,-1,4,6]
Explanation: The queries are processed as follows:
- Query = 2: The interval [2,3] is the smallest interval containing 2. The answer is 3 - 2 + 1 = 2.
- Query = 19: None of the intervals contain 19. The answer is -1.
- Query = 5: The interval [2,5] is the smallest interval containing 5. The answer is 5 - 2 + 1 = 4.
- Query = 22: The interval [20,25] is the smallest interval containing 22. The answer is 25 - 20 + 1 = 6.
```

**Constraints:**

-   `1 <= intervals.length <= $10^5$`
-   `1 <= queries.length <= $10^5$`
-   `intervals[i].length == 2`
-   `1 <= lefti <= righti <= $10^7$`
-   `1 <= queries[j] <= $10^7$`

### Union-Find

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
    vector<int> points, p, w;

    int find(int x) {
        if (x != p[x]) p[x] = find(p[x]);
        return p[x];
    }

    int get(int x) {
        return lower_bound(points.begin(), points.end(), x) - points.begin();
    }

public:
    vector<int> minInterval(vector<vector<int>> &intervals, vector<int> &queries) {
        for (auto &interval: intervals) points.push_back(interval[0]), points.push_back(interval[1]);
        for (int q: queries) points.push_back(q);
        sort(points.begin(), points.end());
        points.erase(unique(points.begin(), points.end()), points.end());
        int n = (int) points.size();
        p.resize(n + 1), w.resize(n + 1, -1);
        for (int i = 0; i < n + 1; i++) p[i] = i;
        sort(intervals.begin(), intervals.end(), [](vector<int> &a, vector<int> &b) {
            return a[1] - a[0] < b[1] - b[0];
        });
        for (auto &interval: intervals) {
            int l = get(interval[0]), r = get(interval[1]), len = interval[1] - interval[0] + 1;
            while (find(l) <= r) {
                l = find(l);
                w[l] = len;
                p[l] = l + 1;
            }
        }
        vector<int> res;
        res.reserve(queries.size());
        for (int q: queries) res.push_back(w[get(q)]);
        return res;
    }
};
```

### multiset

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
    struct Event {
        int type, pos, param;

        bool operator<(const Event &that) const {
            return pos < that.pos || (pos == that.pos && type < that.type);
        }
    };

public:
    vector<int> minInterval(vector<vector<int>> &intervals, vector<int> &queries) {
        int m = (int) queries.size();
        vector<Event> events;
        events.reserve(m);
        for (int i = 0; i < m; i++) {
            events.push_back({1, queries[i], i});
        }
        for (const auto &interval: intervals) {
            events.push_back({0, interval[0], interval[1]});
            events.push_back({2, interval[1], interval[0]});
        }

        sort(events.begin(), events.end());

        vector<int> ans(m, -1);
        multiset<int> seg;
        for (const auto &event: events) {
            if (event.type == 0) {
                seg.insert(event.param - event.pos + 1);
            } else if (event.type == 1) {
                if (!seg.empty()) {
                    ans[event.param] = *seg.begin();
                }
            } else {
                int len = event.pos - event.param + 1;
                auto it = seg.find(len);
                seg.erase(it);
            }
        }
        return ans;
    }
};
```