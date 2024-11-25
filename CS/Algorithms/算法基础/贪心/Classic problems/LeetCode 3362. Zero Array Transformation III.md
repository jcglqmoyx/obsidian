---
page-title: LeetCode 3362. Zero Array Transformation III
url: https://leetcode.com/problems/zero-array-transformation-iii/description/
date: 2024-11-25 18:47:53
---

> 3362\. Zero Array Transformation III

---

You are given an integer array `nums` of length `n` and a 2D array `queries` where `queries[i] = [li, ri]`.

Each `queries[i]` represents the following action on `nums`:

-   Decrement the value at each index in the range `[li, ri]` in `nums` by **at most** 1.
-   The amount by which the value is decremented can be chosen **independently** for each index.

A **Zero Array** is an array with all its elements equal to 0.

Return the **maximum** number of elements that can be removed from `queries`, such that `nums` can still be converted to a **zero array** using the *remaining* queries. If it is not possible to convert `nums` to a **zero array**, return -1.

**Example 1:**

**Input:** nums = \[2,0,2\], queries = \[\[0,2\],\[0,2\],\[1,1\]\]

**Output:** 1

**Explanation:**

After removing `queries[2]`, `nums` can still be converted to a zero array.

-   Using `queries[0]`, decrement `nums[0]` and `nums[2]` by 1 and `nums[1]` by 0.
-   Using `queries[1]`, decrement `nums[0]` and `nums[2]` by 1 and `nums[1]` by 0.

**Example 2:**

**Input:** nums = \[1,1,1,1\], queries = \[\[1,3\],\[0,2\],\[1,3\],\[1,2\]\]

**Output:** 2

**Explanation:**

We can remove `queries[2]` and `queries[3]`.

**Example 3:**

**Input:** nums = \[1,2,3,4\], queries = \[\[0,3\]\]

**Output:** \-1

**Explanation:**

`nums` cannot be converted to a zero array even after using all the queries.

**Constraints:**

-   `1 <= nums.length <= 105`
-   `0 <= nums[i] <= 105`
-   `1 <= queries.length <= 105`
-   `queries[i].length == 2`
-   `0 <= li <= ri < nums.length`

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int maxRemoval(vector<int> &nums, vector<vector<int>> &queries) {  
        sort(queries.begin(), queries.end(), [&](auto &a, auto &b) {  
            return a[0] < b[0];  
        });  
        int n = static_cast<int>(nums.size()), m = static_cast<int>(queries.size());  
        int d[n + 2];  
        memset(d, 0, sizeof d);  
        priority_queue<int> q;  
        for (int i = 1, j = 0; i <= n; i++) {  
            d[i] += d[i - 1];  
            while (j < m && queries[j][0] + 1 <= i) {  
                q.emplace(queries[j][1] + 1);  
                j++;  
            }  
            if (d[i] < nums[i - 1]) {  
                while (d[i] < nums[i - 1]) {  
                    if (q.empty()) {  
                        return -1;  
                    }  
                    int t = q.top();  
                    q.pop();  
                    if (t >= i) {  
                        d[i]++;  
                        d[t + 1]--;  
                    }  
                }  
            }  
        }  
        return static_cast<int> (q.size());  
    }  
};
```