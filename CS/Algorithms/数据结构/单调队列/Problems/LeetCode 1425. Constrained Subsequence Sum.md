---
page-title: "1425. 带限制的子序列和 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/constrained-subsequence-sum/description/?envType=daily-question&envId=2023-10-21
date: "2023-10-21 09:45:40"
---

> [1425\. Constrained Subsequence Sum](https://leetcode.cn/problems/constrained-subsequence-sum/)

---

Given an integer array `nums` and an integer `k`, return the maximum sum of a **non-empty** subsequence of that array such that for every two **consecutive** integers in the subsequence, `nums[i]` and `nums[j]`, where `i < j`, the condition `j - i <= k` is satisfied.

A *subsequence* of an array is obtained by deleting some number of elements (can be zero) from the array, leaving the remaining elements in their original order.

**Example 1:**

**Input:** nums = \[10,2,-10,5,20\], k = 2
**Output:** 37
**Explanation:** The subsequence is \[10, 2, 5, 20\].

**Example 2:**

**Input:** nums = \[-1,-2,-3\], k = 1
**Output:** -1
**Explanation:** The subsequence must be non-empty, so we choose the largest number.

**Example 3:**

**Input:** nums = \[10,-2,-10,-5,20\], k = 2
**Output:** 23
**Explanation:** The subsequence is \[10, -2, -5, 20\].

**Constraints:**

-   `1 <= k <= nums.length <= 105`
-   `-104 <= nums[i] <= 104`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int constrainedSubsetSum(vector<int> &nums, int k) {
        int n = (int) nums.size();
        int f[n];
        memset(f, -0x3f, sizeof f);
        deque<int> dq;
        for (int i = 0; i < n; i++) {
            while (!dq.empty() && i - dq.front() > k) dq.pop_front();
            f[i] = nums[i];
            if (!dq.empty()) f[i] = max(f[i], f[dq.front()] + nums[i]);
            while (!dq.empty() && f[i] >= f[dq.back()]) dq.pop_back();
            dq.emplace_back(i);
        }
        return *max_element(f, f + n);
    }
};
```