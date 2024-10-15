---
page-title: LeetCode 1012. Numbers With Repeated Digits
url: https://leetcode.com/problems/numbers-with-repeated-digits/description/
date: 2024-10-15 15:56:58
---

> 1012\. Numbers With Repeated Digits

---

Given an integer `n`, return *the number of positive integers in the range* `[1, n]` *that have **at least one** repeated digit*.

**Example 1:**

**Input:** n = 20
**Output:** 1
**Explanation:** The only positive number (<= 20) with at least 1 repeated digit is 11.

**Example 2:**

**Input:** n = 100
**Output:** 10
**Explanation:** The positive numbers (<= 100) with atleast 1 repeated digit are 11, 22, 33, 44, 55, 66, 77, 88, 99, and 100.

**Example 3:**

**Input:** n = 1000
**Output:** 262

**Constraints:**

-   `1 <= n <= 109`

```cpp
int f1(int n) {  
    int res = 9;  
    for (int i = 2, op = 9; i <= n; i++, op--) {  
        res *= op;  
    }  
    return res;  
}  
  
int f2(int n, int op) {  
    if (n == 0) return 1;  
    int res = op;  
    op--;  
    for (int i = 2; i <= n; i++, op--) {  
        res *= op;  
    }  
    return res;  
}  
  
class Solution {  
public:  
    int numDupDigitsAtMostN(int n) {  
        int t = n;  
        int d[10], u = 0;  
        while (n) {  
            d[u++] = n % 10;  
            n /= 10;  
        }  
        int res = 0;  
        for (int i = 1; i < u; i++) {  
            res += f1(i);  
        }  
        for (int i = u - 1, mask = 0; i >= 0; i--) {  
            for (int j = i == u - 1 ? 1 : 0; j < d[i]; j++) {  
                if (mask >> j & 1) continue;  
                res += f2(i, 10 - (u - i));  
            }  
            if (mask >> d[i] & 1) break;  
            if (i == 0) res++;  
            mask |= 1 << d[i];  
        }  
        return t - res;  
    }  
};
```