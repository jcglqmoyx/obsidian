---
page-title: LeetCode 862. Shortest Subarray with Sum at Least K
url: https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/description/?envType=daily-question&envId=2024-11-17
date: 2024-11-17 17:15:52
---
Given an integer array `nums` and an integer `k`, return *the length of the shortest non-empty **subarray** of* `nums` *with a sum of at least* `k`. If there is no such **subarray**, return `-1`.

A **subarray** is a **contiguous** part of an array.

**Example 1:**

**Input:** nums = \[1\], k = 1
**Output:** 1

**Example 2:**

**Input:** nums = \[1,2\], k = 4
**Output:** -1

**Example 3:**

**Input:** nums = \[2,-1,2\], k = 3
**Output:** 3

**Constraints:**

-   `1 <= nums.length <= 105`
-   `-105 <= nums[i] <= 105`
-   `1 <= k <= 109`

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int shortestSubarray(vector<int> &nums, int k) {  
        int n = static_cast<int> (nums.size());  
        long long s[n + 1];  
        s[0] = 0;  
        int q[n + 1], hh = 0, tt = -1;  
        q[++tt] = 0;  
        int res = n + 1;  
        for (int i = 1; i <= n; i++) {  
            s[i] = s[i - 1] + nums[i - 1];  
            while (hh <= tt && s[q[tt]] >= s[i]) {  
                tt--;  
            }  
            q[++tt] = i;  
            while (hh <= tt && s[i] - s[q[hh]] >= k) {  
                res = min(res, i - q[hh]);  
                hh++;  
            }  
        }  
        return res == n + 1 ? -1 : res;  
    }  
};
```