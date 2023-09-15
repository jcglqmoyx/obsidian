---
page-title: "1584. 连接所有点的最小费用 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/min-cost-to-connect-all-points/description/?envType=daily-question&envId=2023-09-15
date: "2023-09-15 10:07:14"
---
You are given an array `points` representing integer coordinates of some points on a 2D-plane, where `points[i] = [xi, yi]`.

The cost of connecting two points `[xi, yi]` and `[xj, yj]` is the **manhattan distance** between them: `|xi - xj| + |yi - yj|`, where `|val|` denotes the absolute value of `val`.

Return *the minimum cost to make all points connected.* All points are connected if there is **exactly one** simple path between any two points.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/26/d.png)

**Input:** points = \[\[0,0\],\[2,2\],\[3,10\],\[5,2\],\[7,0\]\]
**Output:** 20
**Explanation:** 
![](https://assets.leetcode.com/uploads/2020/08/26/c.png)
We can connect the points as shown above to get the minimum cost of 20.
Notice that there is a unique path between every pair of points.

**Example 2:**

**Input:** points = \[\[3,12\],\[-2,5\],\[-4,1\]\]
**Output:** 18

**Constraints:**

-   `1 <= points.length <= 1000`
-   `-106 <= xi, yi <= 106`
-   All pairs `(xi, yi)` are distinct.

[官方题解](https://leetcode.cn/problems/min-cost-to-connect-all-points/solutions/565801/lian-jie-suo-you-dian-de-zui-xiao-fei-yo-kcx7/)

O(N^2 * Log(N))
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
    int n;
    vector<int> p;

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

public:
    int minCostConnectPoints(vector<vector<int>> &points) {
        n = (int) points.size();
        p.resize(n);
        iota(p.begin(), p.end(), 0);

        struct Path {
            int from, to;
            int dist;

            bool operator<(const Path &p) const {
                return dist < p.dist;
            }
        };

        vector<Path> v;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                v.push_back({i, j, abs(points[i][0] - points[j][0]) + abs(points[i][1] - points[j][1])});
            }
        }
        sort(v.begin(), v.end());
        int res = 0;
        for (auto &path: v) {
            int pi = find(path.from), pj = find(path.to);
            if (pi != pj) {
                p[pi] = pj;
                res += path.dist;
            }
        }
        return res;
    }
};
```

