---
page-title: LeetCode 3326. Minimum Division Operations to Make Array Non Decreasing
url: https://leetcode.cn/problems/minimum-division-operations-to-make-array-non-decreasing/description/
date: 2024-10-20 15:34:35
---
You are given an integer array `nums`.

Any **positive** divisor of a natural number `x` that is **strictly less** than `x` is called a **proper divisor** of `x`. For example, 2 is a *proper divisor* of 4, while 6 is not a *proper divisor* of 6.

You are allowed to perform an **operation** any number of times on `nums`, where in each **operation** you select any *one* element from `nums` and divide it by its **greatest** **proper divisor**.

Return the **minimum** number of **operations** required to make the array **non-decreasing**.

If it is **not** possible to make the array *non-decreasing* using any number of operations, return `-1`.

**Example 1:**

**Input:** nums = \[25,7\]

**Output:** 1

**Explanation:**

Using a single operation, 25 gets divided by 5 and `nums` becomes `[5, 7]`.

**Example 2:**

**Input:** nums = \[7,7,6\]

**Output:** \-1

**Example 3:**

**Input:** nums = \[1,1,1,1\]

**Output:** 0

**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 106`

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
const int N = 1000001;  
  
int divide_by_proper_divisor[N];  
int pm[N >> 1], idx;  
  
int init = []() {  
    memset(divide_by_proper_divisor, -1, sizeof divide_by_proper_divisor);  
    for (int i = 2; i < N; i++) {  
        if (divide_by_proper_divisor[i] == -1) {  
            pm[idx++] = i;  
            divide_by_proper_divisor[i] = i;  
        }  
        for (int j = 0; pm[j] * i < N; j++) {  
            divide_by_proper_divisor[pm[j] * i] = min(i, pm[j]);  
            if (i % pm[j] == 0) break;  
        }  
    }  
    return 0;  
}();  
  
class Solution {  
public:  
    int minOperations(vector<int> &nums) {  
        int res = 0;  
        for (int mn = 1e9, i = static_cast<int>(nums.size()) - 1; i >= 0; i--) {  
            int x = nums[i];  
            if (x > mn) {  
                x = divide_by_proper_divisor[x];  
                if (x > mn) return -1;  
                res++;  
            }  
            mn = min(mn, x);  
        }  
        return res;  
    }  
};
```