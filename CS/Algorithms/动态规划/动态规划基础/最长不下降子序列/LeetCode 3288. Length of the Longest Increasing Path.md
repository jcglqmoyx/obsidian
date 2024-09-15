---
page-title: LeetCode 3288. Length of the Longest Increasing Path
url: https://leetcode.cn/problems/length-of-the-longest-increasing-path/description/
date: 2024-09-15 23:56:19
---
You are given a 2D array of integers `coordinates` of length `n` and an integer `k`, where `0 <= k < n`.

`coordinates[i] = [xi, yi]` indicates the point `(xi, yi)` in a 2D plane.

An **increasing path** of length `m` is defined as a list of points `(x1, y1)`, `(x2, y2)`, `(x3, y3)`, ..., `(xm, ym)` such that:

-   `xi < xi + 1` and `yi < yi + 1` for all `i` where `1 <= i < m`.
-   `(xi, yi)` is in the given coordinates for all `i` where `1 <= i <= m`.

Return the **maximum** length of an **increasing path** that contains `coordinates[k]`.

**Example 1:**

**Input:** coordinates = \[\[3,1\],\[2,2\],\[4,1\],\[0,0\],\[5,3\]\], k = 1

**Output:** 3

**Explanation:**

`(0, 0)`, `(2, 2)`, `(5, 3)` is the longest increasing path that contains `(2, 2)`.

**Example 2:**

**Input:** coordinates = \[\[2,1\],\[7,0\],\[5,6\]\], k = 2

**Output:** 2

**Explanation:**

`(2, 1)`, `(5, 6)` is the longest increasing path that contains `(5, 6)`.

**Constraints:**

-   `1 <= n == coordinates.length <= 105`
-   `coordinates[i].length == 2`
-   `0 <= coordinates[i][0], coordinates[i][1] <= 109`
-   All elements in `coordinates` are **distinct**.
-   `0 <= k <= n - 1`

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int maxPathLength(vector<vector<int>> &coordinates, int k) {  
        int kx = coordinates[k][0], ky = coordinates[k][1];  
        sort(coordinates.begin(), coordinates.end(), [](auto &a, auto &b) {  
            if (a[0] == b[0]) return a[1] > b[1];  
            return a[0] < b[0];  
        });  
        vector<int> v;  
        for (auto &c: coordinates) {  
            if (c[0] < kx && c[1] < ky || c[0] > kx && c[1] > ky) {  
                auto it = lower_bound(v.begin(), v.end(), c[1]);  
                if (it == v.end()) v.emplace_back(c[1]);  
                else *it = c[1];  
            }  
        }  
        return v.size() + 1;  
    }  
};
```