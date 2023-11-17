---
page-title: "1330. 翻转子数组得到最大的数组值 - 力扣（Leetcode）"
url: https://leetcode.cn/problems/reverse-subarray-to-maximize-array-value/description/
date: "2023-05-12 11:35:43"
---
You are given an integer array `nums`. The *value* of this array is defined as the sum of `|nums[i] - nums[i + 1]|` for all `0 <= i < nums.length - 1`.

You are allowed to select any subarray of the given array and reverse it. You can perform this operation **only once**.

Find maximum possible value of the final array.

**Example 1:**

**Input:** nums = \[2,3,1,5,4\]
**Output:** 10
**Explanation:** By reversing the subarray \[3,1,5\] the array becomes \[2,5,1,3,4\] whose value is 10.

**Example 2:**

**Input:** nums = \[2,4,9,24,2,1,10\]
**Output:** 68

**Constraints:**

-   `1 <= nums.length <= 3 * 104`
-   `-105 <= nums[i] <= 105`
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int maxValueAfterReverse(vector<int> &nums) {
        int n = (int) nums.size();
        int res = 0;
        for (int i = 0; i < n - 1; i++) {
            res += abs(nums[i] - nums[i + 1]);
        }
        int val = res;
        for (int i = 2; i < n; i++) {
            res = max(res, val - abs(nums[i] - nums[i - 1]) + abs(nums[i] - nums[0]));
        }
        for (int i = n - 3; i >= 0; i--) {
            res = max(res, val - abs(nums[i] - nums[i + 1]) + abs(nums[i] - nums[n - 1]));
        }
        int a = max(nums[0], nums[1]), b = min(nums[0], nums[1]);
        for (int i = 0; i < n - 1; i++) {
            a = min(a, max(nums[i], nums[i + 1]));
            b = max(b, min(nums[i], nums[i + 1]));
        }
        res = max(res, val + (b - a) * 2);
        return res;
    }
};
```