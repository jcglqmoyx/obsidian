---
page-title: "1262. 可被三整除的最大和 - 力扣（Leetcode）"
url: https://leetcode.cn/problems/greatest-sum-divisible-by-three/description/
date: "2023-06-19 10:06:05"
---
Given an integer array `nums`, return *the **maximum possible sum** of elements of the array such that it is divisible by three*.

**Example 1:**

**Input:** nums = \[3,6,5,1,8\]
**Output:** 18
**Explanation:** Pick numbers 3, 6, 1 and 8 their sum is 18 (maximum sum divisible by 3).

**Example 2:**

**Input:** nums = \[4\]
**Output:** 0
**Explanation:** Since 4 is not divisible by 3, do not pick any number.

**Example 3:**

**Input:** nums = \[1,2,3,4,4\]
**Output:** 12
**Explanation:** Pick numbers 1, 3, 4 and 4 their sum is 12 (maximum sum divisible by 3).

**Constraints:**

-   `1 <= nums.length <= 4 * 104`
-   `1 <= nums[i] <= 104`
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int maxSumDivThree(vector<int> &nums) {
        int INF = 1e9;
        int f[3] = {0, -INF, -INF};
        for (int x: nums) {
            int g[3];
            memcpy(g, f, sizeof f);
            for (int i = 0; i < 3; i++) {
                g[i] = max(f[i], f[((i - x) % 3 + 3) % 3] + x);
            }
            memcpy(f, g, sizeof g);
        }
        return f[0];
    }
};
```