---
page-title: "2289. 使数组按非递减顺序排列 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/steps-to-make-array-non-decreasing/
date: "2023-10-06 09:48:09"
---
You are given a **0-indexed** integer array `nums`. In one step, **remove** all elements `nums[i]` where `nums[i - 1] > nums[i]` for all `0 < i < nums.length`.

Return *the number of steps performed until* `nums` *becomes a **non-decreasing** array*.

**Example 1:**

**Input:** nums = \[5,3,4,4,7,3,6,11,8,5,11\]
**Output:** 3
**Explanation:** The following are the steps performed:
- Step 1: \[5,**3**,4,4,7,**3**,6,11,**8**,**5**,11\] becomes \[5,4,4,7,6,11,11\]
- Step 2: \[5,**4**,4,7,**6**,11,11\] becomes \[5,4,7,11,11\]
- Step 3: \[5,**4**,7,11,11\] becomes \[5,7,11,11\]
\[5,7,11,11\] is a non-decreasing array. Therefore, we return 3.

**Example 2:**

**Input:** nums = \[4,5,7,7,13\]
**Output:** 0
**Explanation:** nums is already a non-decreasing array. Therefore, we return 0.

**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 109`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int totalSteps(vector<int> &nums) {
        int ans = 0;
        stack<pair<int, int>> st;
        for (int num: nums) {
            int t = 0;
            while (!st.empty() && st.top().first <= num) {
                t = max(t, st.top().second);
                st.pop();
            }
            t = st.empty() ? 0 : t + 1;
            ans = max(ans, t);
            st.emplace(num, t);
        }
        return ans;
    }
};
```