---
page-title: "2009. 使数组连续的最少操作数 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/minimum-number-of-operations-to-make-array-continuous/description/?envType=daily-question&envId=2023-10-10
date: "2023-10-10 16:30:40"
---
You are given an integer array `nums`. In one operation, you can replace **any** element in `nums` with **any** integer.

`nums` is considered **continuous** if both of the following conditions are fulfilled:

-   All elements in `nums` are **unique**.
-   The difference between the **maximum** element and the **minimum** element in `nums` equals `nums.length - 1`.

For example, `nums = [4, 2, 5, 3]` is **continuous**, but `nums = [1, 2, 3, 5, 6]` is **not continuous**.

Return *the **minimum** number of operations to make* `nums` ***continuous***.

**Example 1:**

**Input:** nums = \[4,2,5,3\]
**Output:** 0
**Explanation:** nums is already continuous.

**Example 2:**

**Input:** nums = \[1,2,3,5,6\]
**Output:** 1
**Explanation:** One possible solution is to change the last element to 4.
The resulting array is \[1,2,3,5,4\], which is continuous.

**Example 3:**

**Input:** nums = \[1,10,100,1000\]
**Output:** 3
**Explanation:** One possible solution is to:
- Change the second element to 2.
- Change the third element to 3.
- Change the fourth element to 4.
The resulting array is \[1,2,3,4\], which is continuous.

**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 109`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int minOperations(vector<int> &nums) {
        sort(nums.begin(), nums.end());
        int n = (int) nums.size(), res = n;
        nums.erase(unique(nums.begin(), nums.end()), nums.end());
        for (int l = 0, r = 0; l < nums.size(); l++) {
            while (r < nums.size() && nums[r] - nums[l] + 1 <= n) r++;
            res = min(res, n - (r - l));
        }
        return res;
    }
};
```