---
page-title: LeetCode 3336. Find the Number of Subsequences With Equal GCD
url: https://leetcode.com/problems/find-the-number-of-subsequences-with-equal-gcd/description/
date: 2024-10-27 22:26:26
---
You are given an integer array `nums`.

Your task is to find the number of pairs of **non-empty** subsequences `(seq1, seq2)` of `nums` that satisfy the following conditions:

-   The subsequences `seq1` and `seq2` are **disjoint**, meaning **no index** of `nums` is common between them.
-   The GCD of the elements of `seq1` is equal to the GCD of the elements of `seq2`.

Return the total number of such pairs.

Since the answer may be very large, return it **modulo** `109 + 7`.

The term `gcd(a, b)` denotes the **greatest common divisor** of `a` and `b`.

A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** nums = \[1,2,3,4\]

**Output:** 10

**Explanation:**

The subsequence pairs which have the GCD of their elements equal to 1 are:

-   `([**1**, 2, 3, 4], [1, **2**, **3**, 4])`
-   `([**1**, 2, 3, 4], [1, **2**, **3**, **4**])`
-   `([**1**, 2, 3, 4], [1, 2, **3**, **4**])`
-   `([**1**, **2**, 3, 4], [1, 2, **3**, **4**])`
-   `([**1**, 2, 3, **4**], [1, **2**, **3**, 4])`
-   `([1, **2**, **3**, 4], [**1**, 2, 3, 4])`
-   `([1, **2**, **3**, 4], [**1**, 2, 3, **4**])`
-   `([1, **2**, **3**, **4**], [**1**, 2, 3, 4])`
-   `([1, 2, **3**, **4**], [**1**, 2, 3, 4])`
-   `([1, 2, **3**, **4**], [**1**, **2**, 3, 4])`

**Example 2:**

**Input:** nums = \[10,20,30\]

**Output:** 2

**Explanation:**

The subsequence pairs which have the GCD of their elements equal to 10 are:

-   `([**10**, 20, 30], [10, **20**, **30**])`
-   `([10, **20**, **30**], [**10**, 20, 30])`

**Example 3:**

**Input:** nums = \[1,1,1,1\]

**Output:** 50

**Constraints:**

-   `1 <= nums.length <= 200`
-   `1 <= nums[i] <= 200`

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int subsequencePairCount(vector<int> &nums) {  
        int n = static_cast<int>(nums.size()), m = *max_element(nums.begin(), nums.end()), MOD = 1e9 + 7;  
        int f[n][m + 1][m + 1];  
        memset(f, -1, sizeof f);  
        auto dp = [&](auto &&dp, int u, int x, int y) -> int {  
            if (u == n) {  
                return x == y;  
            }  
            if (f[u][x][y] != -1) return f[u][x][y];  
            int &res = f[u][x][y];  
            res = (dp(dp, u + 1, gcd(x, nums[u]), y) + dp(dp, u + 1, x, gcd(y, nums[u]))) % MOD;  
            res = (res + dp(dp, u + 1, x, y)) % MOD;  
            return res;  
        };  
        return (dp(dp, 0, 0, 0) - 1 + MOD) % MOD;  
    }  
};
```