---
page-title: "260. 只出现一次的数字 III - 力扣（LeetCode）"
url: https://leetcode.cn/problems/single-number-iii/?envType=daily-question&envId=2023-10-16
date: "2023-10-16 09:40:10"
---

> [260\. Single Number III](https://leetcode.cn/problems/single-number-iii/)

---

Given an integer array `nums`, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once. You can return the answer in **any order**.

You must write an algorithm that runs in linear runtime complexity and uses only constant extra space.

**Example 1:**

**Input:** nums = \[1,2,1,3,2,5\]
**Output:** \[3,5\]
**Explanation: ** \[5, 3\] is also a valid answer.

**Example 2:**

**Input:** nums = \[-1,0\]
**Output:** \[-1,0\]

**Example 3:**

**Input:** nums = \[0,1\]
**Output:** \[1,0\]

**Constraints:**

-   `2 <= nums.length <= 3 * 104`
-   `-231 <= nums[i] <= 231 - 1`
-   Each integer in `nums` will appear twice, only two integers will appear once.

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    vector<int> singleNumber(vector<int> &nums) {
        int s = 0;
        for (int x: nums) {
            s ^= x;
        }
        s = s == INT32_MIN ? 1 : s & -s;
        int a = 0, b = 0;
        for (int x: nums) {
            if (x & s) {
                a ^= x;
            } else {
                b ^= x;
            }
        }
        return {a, b};
    }
};
```