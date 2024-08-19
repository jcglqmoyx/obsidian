---
page-title: LeetCode 264. Ugly Number II
url: https://leetcode.com/problems/ugly-number-ii/description/?envType=daily-question&envId=2024-08-18
date: 2024-08-18 10:19:38
---
---

An **ugly number** is a positive integer whose prime factors are limited to `2`, `3`, and `5`.  
  
Given an integer `n`, return *the* `nth` ***ugly number***.  
  
**Example 1:**  
  
**Input:** n = 10  
**Output:** 12  
**Explanation:** \[1, 2, 3, 4, 5, 6, 8, 9, 10, 12\] is the sequence of the first 10 ugly numbers.  
  
**Example 2:**  
  
**Input:** n = 1  
**Output:** 1  
**Explanation:** 1 has no prime factors, therefore all of its prime factors are limited to 2, 3, and 5.  
  
**Constraints:**  
  
-   `1 <= n <= 1690`

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int nthUglyNumber(int n) {  
        int dp[n + 1];  
        dp[1] = 1;  
        int p2 = 1, p3 = 1, p5 = 1;  
        for (int i = 2; i <= n; i++) {  
            int num2 = dp[p2] * 2, num3 = dp[p3] * 3, num5 = dp[p5] * 5;  
            dp[i] = min({num2, num3, num5});  
            if (dp[i] == num2) {  
                p2++;  
            }  
            if (dp[i] == num3) {  
                p3++;  
            }  
            if (dp[i] == num5) {  
                p5++;  
            }  
        }  
        return dp[n];  
    }  
}
```