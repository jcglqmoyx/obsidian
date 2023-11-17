---
page-title: "1793. 好子数组的最大分数 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/maximum-score-of-a-good-subarray/description/?envType=daily-question&envId=2023-10-22
date: "2023-10-22 13:18:50"
---
You are given an array of integers `nums` **(0-indexed)** and an integer `k`.

The **score** of a subarray `(i, j)` is defined as `min(nums[i], nums[i+1], ..., nums[j]) * (j - i + 1)`. A **good** subarray is a subarray where `i <= k <= j`.

Return *the maximum possible **score** of a **good** subarray.*

**Example 1:**

**Input:** nums = \[1,4,3,7,4,5\], k = 3
**Output:** 15
**Explanation:** The optimal subarray is (1, 5) with a score of min(4,3,7,4,5) \* (5-1+1) = 3 \* 5 = 15. 

**Example 2:**

**Input:** nums = \[5,5,4,5,4,1,1,1\], k = 0
**Output:** 20
**Explanation:** The optimal subarray is (0, 4) with a score of min(5,5,4,5,4) \* (4-0+1) = 4 \* 5 = 20.

**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 2 * 104`
-   `0 <= k < nums.length`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int maximumScore(vector<int> &nums, int k) {
        int n = (int) nums.size();
        int r[n];
        stack<int> stk;
        for (int i = n - 1; i >= 0; i--) {
            while (!stk.empty() && nums[stk.top()] >= nums[i]) stk.pop();
            r[i] = stk.empty() ? n - 1 : stk.top() - 1;
            stk.emplace(i);
        }
        int res = 0;
        stk = stack<int>();
        for (int i = 0; i < n; i++) {
            while (!stk.empty() && nums[stk.top()] >= nums[i]) stk.pop();
            int l = stk.empty() ? 0 : stk.top() + 1;
            if (l <= k && r[i] >= k) {
                res = max(res, nums[i] * (r[i] - l + 1));
            }
            stk.emplace(i);
        }
        return res;
    }
};
```