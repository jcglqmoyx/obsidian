---
page-title: "2104. 子数组范围和 - 力扣（Leetcode）"
url: https://leetcode.cn/problems/sum-of-subarray-ranges/description/
date: "2023-06-19 11:07:57"
---

> You are given an integer array `nums`. The **range** of a subarray of `nums` is the difference between the largest and smallest element in the subarray.
> 
> Return *the **sum of all** subarray ranges of* `nums`*.*
> 
> A subarray is a contiguous **non-empty** sequence of elements within an array.
> 
> **Example 1:**
> 
> **Input:** nums = \[1,2,3\]
> **Output:** 4
> **Explanation:** The 6 subarrays of nums are the following:
> \[1\], range = largest - smallest = 1 - 1 = 0 
> \[2\], range = 2 - 2 = 0
> \[3\], range = 3 - 3 = 0
> \[1,2\], range = 2 - 1 = 1
> \[2,3\], range = 3 - 2 = 1
> \[1,2,3\], range = 3 - 1 = 2
> So the sum of all ranges is 0 + 0 + 0 + 1 + 1 + 2 = 4.
> 
> **Example 2:**
> 
> **Input:** nums = \[1,3,3\]
> **Output:** 4
> **Explanation:** The 6 subarrays of nums are the following:
> \[1\], range = largest - smallest = 1 - 1 = 0
> \[3\], range = 3 - 3 = 0
> \[3\], range = 3 - 3 = 0
> \[1,3\], range = 3 - 1 = 2
> \[3,3\], range = 3 - 3 = 0
> \[1,3,3\], range = 3 - 1 = 2
> So the sum of all ranges is 0 + 0 + 0 + 2 + 0 + 2 = 4.
> 
> **Example 3:**
> 
> **Input:** nums = \[4,-2,-3,4,1\]
> **Output:** 59
> **Explanation:** The sum of all subarray ranges of nums is 59.
> 
> **Constraints:**
> 
> -   `1 <= nums.length <= 1000`
> -   `-109 <= nums[i] <= 109`
> 
> **Follow-up:** Could you find a solution with `O(n)` time complexity?

---

You are given an integer array `nums`. The **range** of a subarray of `nums` is the difference between the largest and smallest element in the subarray.

Return *the **sum of all** subarray ranges of* `nums`*.*

A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = \[1,2,3\]
**Output:** 4
**Explanation:** The 6 subarrays of nums are the following:
\[1\], range = largest - smallest = 1 - 1 = 0 
\[2\], range = 2 - 2 = 0
\[3\], range = 3 - 3 = 0
\[1,2\], range = 2 - 1 = 1
\[2,3\], range = 3 - 2 = 1
\[1,2,3\], range = 3 - 1 = 2
So the sum of all ranges is 0 + 0 + 0 + 1 + 1 + 2 = 4.

**Example 2:**

**Input:** nums = \[1,3,3\]
**Output:** 4
**Explanation:** The 6 subarrays of nums are the following:
\[1\], range = largest - smallest = 1 - 1 = 0
\[3\], range = 3 - 3 = 0
\[3\], range = 3 - 3 = 0
\[1,3\], range = 3 - 1 = 2
\[3,3\], range = 3 - 3 = 0
\[1,3,3\], range = 3 - 1 = 2
So the sum of all ranges is 0 + 0 + 0 + 2 + 0 + 2 = 4.

**Example 3:**

**Input:** nums = \[4,-2,-3,4,1\]
**Output:** 59
**Explanation:** The sum of all subarray ranges of nums is 59.

**Constraints:**

-   `1 <= nums.length <= 1000`
-   `-109 <= nums[i] <= 109`

**Follow-up:** Could you find a solution with `O(n)` time complexity?

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    long long subArrayRanges(vector<int> &nums) {
        using LL = long long;

        int n = (int) nums.size();
        vector<int> l(n), r(n);
        stack<int> stk;
        for (int i = 0; i < n; i++) {
            while (!stk.empty() && nums[stk.top()] >= nums[i]) stk.pop();
            if (stk.empty()) l[i] = -1;
            else l[i] = stk.top();
            stk.push(i);
        }
        while (!stk.empty()) stk.pop();
        for (int i = n - 1; ~i; i--) {
            while (!stk.empty() && nums[stk.top()] > nums[i]) stk.pop();
            if (stk.empty()) r[i] = n;
            else r[i] = stk.top();
            stk.push(i);
        }
        LL res = 0;
        for (int i = 0; i < n; i++) {
            res -= (LL) nums[i] * (i - l[i]) * (r[i] - i);
        }
        while (!stk.empty()) stk.pop();
        for (int i = 0; i < n; i++) {
            while (!stk.empty() && nums[stk.top()] <= nums[i]) stk.pop();
            if (stk.empty()) l[i] = -1;
            else l[i] = stk.top();
            stk.push(i);
        }
        while (!stk.empty()) stk.pop();
        for (int i = n - 1; ~i; i--) {
            while (!stk.empty() && nums[stk.top()] < nums[i]) stk.pop();
            if (stk.empty()) r[i] = n;
            else r[i] = stk.top();
            stk.push(i);
        }
        for (int i = 0; i < n; i++) {
            res += (LL) nums[i] * (i - l[i]) * (r[i] - i);
        }
        return res;
    }
};
```