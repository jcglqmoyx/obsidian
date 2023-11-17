---
page-title: "456. 132 模式 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/132-pattern/description/?envType=daily-question&envId=2023-09-30
date: "2023-09-30 12:28:14"
---

> 456\. 132 Pattern

---

Given an array of `n` integers `nums`, a **132 pattern** is a subsequence of three integers `nums[i]`, `nums[j]` and `nums[k]` such that `i < j < k` and `nums[i] < nums[k] < nums[j]`.

Return `true` *if there is a **132 pattern** in* `nums`*, otherwise, return* `false`*.*

**Example 1:**

**Input:** nums = \[1,2,3,4\]
**Output:** false
**Explanation:** There is no 132 pattern in the sequence.

**Example 2:**

**Input:** nums = \[3,1,4,2\]
**Output:** true
**Explanation:** There is a 132 pattern in the sequence: \[1, 4, 2\].

**Example 3:**

**Input:** nums = \[-1,3,2,0\]
**Output:** true
**Explanation:** There are three 132 patterns in the sequence: \[-1, 3, 2\], \[-1, 3, 0\] and \[-1, 2, 0\].

**Constraints:**

-   `n == nums.length`
-   `1 <= n <= 2 * 105`
-   `-109 <= nums[i] <= 109`
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    bool find132pattern(vector<int> &nums) {
        int n = (int) nums.size();
        if (n < 3) return false;
        stack<int> stk;
        stk.emplace(nums.back());
        int mx = -1e9;
        for (int i = n - 2; i >= 0; i--) {
            if (nums[i] < mx) return true;
            while (!stk.empty() && nums[i] > stk.top()) {
                mx = stk.top();
                stk.pop();
            }
            if (nums[i] > mx) {
                stk.push(nums[i]);
            }
        }
        return false;
    }
};
```