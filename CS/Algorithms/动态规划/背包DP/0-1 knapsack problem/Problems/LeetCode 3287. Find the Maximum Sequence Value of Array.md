---
page-title: LeetCode 3287. Find the Maximum Sequence Value of Array
url: https://leetcode.cn/problems/find-the-maximum-sequence-value-of-array/description/
date: 2024-09-15 22:24:59
---

> 3287\. Find the Maximum Sequence Value of Array

---

You are given an integer array `nums` and a **positive** integer `k`.

The **value** of a sequence `seq` of size `2 * x` is defined as:

-   `(seq[0] OR seq[1] OR ... OR seq[x - 1]) XOR (seq[x] OR seq[x + 1] OR ... OR seq[2 * x - 1])`.

Return the **maximum** **value** of any subsequence of `nums` having size `2 * k`.

**Example 1:**

**Input:** nums = \[2,6,7\], k = 1

**Output:** 5

**Explanation:**

The subsequence `[2, 7]` has the maximum value of `2 XOR 7 = 5`.

**Example 2:**

**Input:** nums = \[4,2,5,6,7\], k = 2

**Output:** 2

**Explanation:**

The subsequence `[4, 5, 6, 7]` has the maximum value of `(4 OR 5) XOR (6 OR 7) = 2`.

**Constraints:**

-   `2 <= nums.length <= 400`
-   `1 <= nums[i] < 27`
-   `1 <= k <= nums.length / 2`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int maxValue(vector<int> &nums, int k) {
        int n = nums.size();
        int mx = 1 << 7;
        bool l[n - k + 1][mx];
        memset(l, 0, sizeof l);
        int f[k + 1][mx];
        memset(f, 0, sizeof f);
        f[0][0] = true;
        for (int i = 1; i + k - 1 < n; i++) {
            for (int j = min(k - 1, i); j >= 0; j--) {
                for (int num = 0; num < mx; num++) {
                    if (f[j][num]) f[j + 1][num | nums[i - 1]] = true;
                }
            }
            for (int num = 1; num < mx; num++) {
                l[i][num] = f[k][num];
            }
        }
        memset(f, 0, sizeof f);
        f[0][0] = true;
        int res = 0;
        for (int i = n; i > k; i--) {
            for (int j = min(k - 1, n - i + 1); j >= 0; j--) {
                for (int num = 0; num < mx; num++) {
                    if (f[j][num]) f[j + 1][num | nums[i - 1]] = true;
                }
            }
            for (int right = 1; right < mx; right++) {
                if (f[k][right]) {
                    for (int left = 1; left < mx; left++) {
                        if (l[i - 1][left]) {
                            res = max(res, left ^ right);
                        }
                    }
                }
            }
        }
        return res;
    }
};
```