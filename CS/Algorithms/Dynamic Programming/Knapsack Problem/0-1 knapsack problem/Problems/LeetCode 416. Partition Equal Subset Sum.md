---
page-title: "416. 分割等和子集 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/partition-equal-subset-sum/
date: "2023-10-22 13:43:46"
---

> [416\. Partition Equal Subset Sum](https://leetcode.cn/problems/partition-equal-subset-sum/)

---

Given an integer array `nums`, return `true` *if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or* `false` *otherwise*.

**Example 1:**

**Input:** nums = \[1,5,11,5\]
**Output:** true
**Explanation:** The array can be partitioned as \[1, 5, 5\] and \[11\].

**Example 2:**

**Input:** nums = \[1,2,3,5\]
**Output:** false
**Explanation:** The array cannot be partitioned into equal sum subsets.

**Constraints:**

-   `1 <= nums.length <= 200`
-   `1 <= nums[i] <= 100`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    bool canPartition(vector<int> &nums) {
        int s = 0;
        for (int x: nums) s += x;
        if (s & 1) return false;
        s >>= 1;
        bool f[s + 1];
        memset(f, 0, sizeof f);
        f[0] = true;
        for (int x: nums) {
            if (x <= s) {
                for (int i = s; i >= x; i--) {
                    f[i] |= f[i - x];
                }
            }
        }
        return f[s];
    }
};
```