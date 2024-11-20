---
page-title: LeetCode 3244. Shortest Distance After Road Addition Queries II
url: https://leetcode.cn/problems/shortest-distance-after-road-addition-queries-ii/description/?envType=daily-question&envId=2024-11-20
date: 2024-11-20 11:17:18
---
You are given an integer `n` and a 2D integer array `queries`.

There are `n` cities numbered from `0` to `n - 1`. Initially, there is a **unidirectional** road from city `i` to city `i + 1` for all `0 <= i < n - 1`.

`queries[i] = [ui, vi]` represents the addition of a new **unidirectional** road from city `ui` to city `vi`. After each query, you need to find the **length** of the **shortest path** from city `0` to city `n - 1`.

There are no two queries such that `queries[i][0] < queries[j][0] < queries[i][1] < queries[j][1]`.

Return an array `answer` where for each `i` in the range `[0, queries.length - 1]`, `answer[i]` is the *length of the shortest path* from city `0` to city `n - 1` after processing the **first** `i + 1` queries.

**Example 1:**

**Input:** n = 5, queries = \[\[2,4\],\[0,2\],\[0,4\]\]

**Output:** \[3,2,1\]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/06/28/image8.jpg)

After the addition of the road from 2 to 4, the length of the shortest path from 0 to 4 is 3.

![](https://assets.leetcode.com/uploads/2024/06/28/image9.jpg)

After the addition of the road from 0 to 2, the length of the shortest path from 0 to 4 is 2.

![](https://assets.leetcode.com/uploads/2024/06/28/image10.jpg)

After the addition of the road from 0 to 4, the length of the shortest path from 0 to 4 is 1.

**Example 2:**

**Input:** n = 4, queries = \[\[0,3\],\[0,2\]\]

**Output:** \[1,1\]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/06/28/image11.jpg)

After the addition of the road from 0 to 3, the length of the shortest path from 0 to 3 is 1.

![](https://assets.leetcode.com/uploads/2024/06/28/image12.jpg)

After the addition of the road from 0 to 2, the length of the shortest path remains 1.

**Constraints:**

-   `3 <= n <= 105`
-   `1 <= queries.length <= 105`
-   `queries[i].length == 2`
-   `0 <= queries[i][0] < queries[i][1] < n`
-   `1 < queries[i][1] - queries[i][0]`
-   There are no repeated roads among the queries.
-   There are no two queries such that `i != j` and `queries[i][0] < queries[j][0] < queries[i][1] < queries[j][1]`.

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    vector<int> shortestDistanceAfterQueries(int n, vector<vector<int>> &queries) {  
        int ne[n];  
        iota(ne, ne + n, 1);  
        auto m = queries.size();  
        vector<int> res(m);  
        for (int dist = n - 1, i = 0; i < m; i++) {  
            int u = queries[i][0], v = queries[i][1];  
            if (ne[u] && ne[u] < v) {  
                int j = u + 1;  
                while (j && j < v) {  
                    int nj = ne[j];  
                    ne[j] = 0;  
                    j = nj;  
                    dist--;  
                }  
                ne[u] = v;  
            }  
            res[i] = dist;  
        }  
        return res;  
    }  
};
```