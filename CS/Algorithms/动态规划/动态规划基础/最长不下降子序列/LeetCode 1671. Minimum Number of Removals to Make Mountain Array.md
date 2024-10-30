---
page-title: LeetCode 1671. Minimum Number of Removals to Make Mountain Array
url: https://leetcode.com/problems/minimum-number-of-removals-to-make-mountain-array/description/?envType=daily-question&envId=2024-10-30
date: 2024-10-30 11:20:33
---
You may recall that an array `arr` is a **mountain array** if and only if:

-   `arr.length >= 3`
-   There exists some index `i` (**0-indexed**) with `0 < i < arr.length - 1` such that:
    -   `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]`
    -   `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`

Given an integer array `nums`​​​, return *the **minimum** number of elements to remove to make* `nums*​​​*` *a **mountain array**.*

**Example 1:**

**Input:** nums = \[1,3,1\]
**Output:** 0
**Explanation:** The array itself is a mountain array so we do not need to remove any elements.

**Example 2:**

**Input:** nums = \[2,1,1,5,6,2,3,1\]
**Output:** 3
**Explanation:** One solution is to remove the elements at indices 0, 1, and 5, making the array nums = \[1,5,6,3,1\].

**Constraints:**

-   `3 <= nums.length <= 1000`
-   `1 <= nums[i] <= 109`
-   It is guaranteed that you can make a mountain array out of `nums`.

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int minimumMountainRemovals(vector<int> &nums) {  
        int n = static_cast<int>(nums.size());  
        int l[n], v[n];  
        for (int idx = 0, i = 0; i < n; i++) {  
            if (idx == 0 || nums[i] > v[idx - 1]) {  
                v[idx++] = nums[i];  
                l[i] = idx;  
            } else {  
                int p = static_cast<int>((lower_bound(v, v + idx, nums[i]) - v));  
                v[p] = nums[i];  
                l[i] = p + 1;  
            }  
        }  
        int res = n;  
        for (int idx = 0, i = n - 1; i >= 0; i--) {  
            int ri;  
            if (idx == 0 || nums[i] > v[idx - 1]) {  
                v[idx++] = nums[i];  
                ri = idx;  
            } else {  
                int p = static_cast<int>((lower_bound(v, v + idx, nums[i]) - v));  
                v[p] = nums[i];  
                ri = p + 1;  
            }  
            if (l[i] > 1 && ri > 1) {  
                res = min(res, n - l[i] - ri + 1);  
            }  
        }  
        return res;  
    }  
};
```