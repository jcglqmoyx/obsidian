---
page-title: "962. 最大宽度坡 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/maximum-width-ramp/description/
date: "2023-10-04 10:51:35"
---
A **ramp** in an integer array `nums` is a pair `(i, j)` for which `i < j` and `nums[i] <= nums[j]`. The **width** of such a ramp is `j - i`.

Given an integer array `nums`, return *the maximum width of a **ramp** in* `nums`. If there is no **ramp** in `nums`, return `0`.

**Example 1:**

**Input:** nums = \[6,0,8,2,1,5\]
**Output:** 4
**Explanation:** The maximum width ramp is achieved at (i, j) = (1, 5): nums\[1\] = 0 and nums\[5\] = 5.

**Example 2:**

**Input:** nums = \[9,8,1,0,1,9,4,0,4,1\]
**Output:** 7
**Explanation:** The maximum width ramp is achieved at (i, j) = (2, 9): nums\[2\] = 1 and nums\[9\] = 1.

**Constraints:**

-   `2 <= nums.length <= 5 * 104`
-   `0 <= nums[i] <= 5 * 104`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int maxWidthRamp(vector<int> &nums) {
        int n = (int) nums.size(), res = 0;
        stack<int> stk;
        for (int i = 0; i < n; i++) {
            if (stk.empty() || nums[stk.top()] > nums[i]) {
                stk.push(i);
            }
        }
        for (int i = n - 1; i > 0; i--) {
            while (!stk.empty() && nums[i] >= nums[stk.top()]) {
                res = max(res, i - stk.top());
                stk.pop();
            }
        }
        return res;
    }
};
```