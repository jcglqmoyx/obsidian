---
page-title: LeetCode 1203. Sort Items by Groups Respecting Dependencies
url: https://leetcode.com/problems/sort-items-by-groups-respecting-dependencies/description/
date: 2024-12-02 15:57:55
---
There are `n` items each belonging to zero or one of `m` groups where `group[i]` is the group that the `i`\-th item belongs to and it's equal to `-1` if the `i`\-th item belongs to no group. The items and the groups are zero indexed. A group can have no item belonging to it.

Return a sorted list of the items such that:

-   The items that belong to the same group are next to each other in the sorted list.
-   There are some relations between these items where `beforeItems[i]` is a list containing all the items that should come before the `i`\-th item in the sorted array (to the left of the `i`\-th item).

Return any solution if there is more than one solution and return an **empty list** if there is no solution.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2019/09/11/1359_ex1.png)**

**Input:** n = 8, m = 2, group = \[-1,-1,1,0,0,1,0,-1\], beforeItems = \[\[\],\[6\],\[5\],\[6\],\[3,6\],\[\],\[\],\[\]\]
**Output:** \[6,3,4,1,5,2,0,7\]

**Example 2:**

**Input:** n = 8, m = 2, group = \[-1,-1,1,0,0,1,0,-1\], beforeItems = \[\[\],\[6\],\[5\],\[6\],\[3\],\[\],\[4\],\[\]\]
**Output:** \[\]
**Explanation:** This is the same as example 1 except that 4 needs to be before 6 in the sorted list.

**Constraints:**

-   `1 <= m <= n <= 3 * 104`
-   `group.length == beforeItems.length == n`
-   `-1 <= group[i] <= m - 1`
-   `0 <= beforeItems[i].length <= n - 1`
-   `0 <= beforeItems[i][j] <= n - 1`
-   `i != beforeItems[i][j]`
-   `beforeItems[i]` does not contain duplicates elements.

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    vector<int> sortItems(int n, int m, vector<int> &group, vector<vector<int>> &beforeItems) {  
        vector<vector<int>> g_group(m);  
        vector<int> d_group(m);  
        vector<vector<int>> group_elements(m);  
        for (int i = 0; i < n; i++) {  
            if (group[i] == -1) {  
                group[i] = m++;  
                d_group.emplace_back(0);  
                g_group.emplace_back();  
                group_elements.emplace_back();  
            }  
        }  
        for (int i = 0; i < n; i++) {  
            for (int x: beforeItems[i]) {  
                if (group[x] != group[i]) {  
                    d_group[group[i]]++;  
                    g_group[group[x]].emplace_back(group[i]);  
                }  
            }  
        }  
        int q[n * 2];  
        int hh = 0, tt = -1;  
        int group_path[m], idx = 0;  
        for (int j = 0; j < m; j++) {  
            if (d_group[j] == 0) {  
                q[++tt] = j;  
            }  
        }  
        while (hh <= tt) {  
            int t = q[hh++];  
            group_path[idx++] = t;  
            for (int x: g_group[t]) {  
                if (--d_group[x] == 0) {  
                    q[++tt] = x;  
                }  
            }  
        }  
        if (idx != m) {  
            return {};  
        }  
        vector<vector<int>> g_item(n);  
        int d_item[n];  
        memset(d_item, 0, sizeof d_item);  
        for (int i = 0; i < n; i++) {  
            for (int x: beforeItems[i]) {  
                g_item[x].emplace_back(i);  
                d_item[i]++;  
            }  
        }  
        hh = 0, tt = -1;  
        for (int i = 0; i < n; i++) {  
            if (d_item[i] == 0) {  
                q[++tt] = i;  
            }  
        }  
        while (hh <= tt) {  
            int t = q[hh++];  
            group_elements[group[t]].emplace_back(t);  
            for (int x: g_item[t]) {  
                if (--d_item[x] == 0) {  
                    q[++tt] = x;  
                }  
            }  
        }  
        vector<int> res;  
        for (int i = 0; i < idx; i++) {  
            for (int x: group_elements[group_path[i]]) {  
                res.emplace_back(x);  
            }  
        }  
        if (res.size() != n) {  
            res.clear();  
        }  
        return res;  
    }  
};
```