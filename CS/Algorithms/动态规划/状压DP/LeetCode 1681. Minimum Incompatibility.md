---
page-title: LeetCode 1681. Minimum Incompatibility
url: https://leetcode.com/problems/minimum-incompatibility/description/
date: 2024-09-20 14:31:58
---
You are given an integer array `nums`​​​ and an integer `k`. You are asked to distribute this array into `k` subsets of **equal size** such that there are no two equal elements in the same subset.

A subset's **incompatibility** is the difference between the maximum and minimum elements in that array.

Return *the **minimum possible sum of incompatibilities** of the* `k` *subsets after distributing the array optimally, or return* `-1` *if it is not possible.*

A subset is a group integers that appear in the array with no particular order.

**Example 1:**

**Input:** nums = \[1,2,1,4\], k = 2
**Output:** 4
**Explanation:** The optimal distribution of subsets is \[1,2\] and \[1,4\].
The incompatibility is (2-1) + (4-1) = 4.
Note that \[1,1\] and \[2,4\] would result in a smaller sum, but the first subset contains 2 equal elements.

**Example 2:**

**Input:** nums = \[6,3,8,1,3,1,2,2\], k = 4
**Output:** 6
**Explanation:** The optimal distribution of subsets is \[1,2\], \[2,3\], \[6,8\], and \[1,3\].
The incompatibility is (2-1) + (3-2) + (8-6) + (3-1) = 6.

**Example 3:**

**Input:** nums = \[5,3,3,6,3,3\], k = 3
**Output:** -1
**Explanation:** It is impossible to distribute nums into 3 subsets where no two elements are equal in the same subset.

**Constraints:**

-   `1 <= k <= nums.length <= 16`
-   `nums.length` is divisible by `k`
-   `1 <= nums[i] <= nums.length`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int minimumIncompatibility(vector<int> &nums, int k) {
        auto n = nums.size();
        if (n == 1) return 0;
        int sub = n / k;
        auto check = [&](int mask) {
            if (__builtin_popcount(mask) != sub) return -1;
            int used = 0;
            int mn = n, mx = 1;
            for (int i = 0; i < n; i++) {
                if (mask >> i & 1) {
                    if (used >> nums[i] & 1) return -1;
                    used |= 1 << nums[i];
                    mn = min(mn, nums[i]);
                    mx = max(mx, nums[i]);
                }
            }
            return mx - mn;
        };
        int valid[1 << n];
        for (int i = 0; i < 1 << n; i++) {
            valid[i] = check(i);
        }
        int f[1 << n], inf = 0x3f3f3f3f;
        memset(f, 0x3f, sizeof f);
        f[0] = 0;
        for (int i = 1; i < 1 << n; i++) {
            if (__builtin_popcount(i) % sub == 0) {
                for (int j = i; j; j = (j - 1) & i) {
                    if (valid[j] != -1) {
                        f[i] = min(f[i], f[i ^ j] + valid[j]);
                    }
                }
            }
        }
        int res = f[(1 << n) - 1];
        if (res >= inf) res = -1;
        return res;
    }
};
```