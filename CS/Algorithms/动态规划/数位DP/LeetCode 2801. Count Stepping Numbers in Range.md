---
page-title: LeetCode 2801. Count Stepping Numbers in Range
url: https://leetcode.com/problems/count-stepping-numbers-in-range/description/
date: 2024-10-17 20:39:50
---
Given two positive integers `low` and `high` represented as strings, find the count of **stepping numbers** in the inclusive range `[low, high]`.

A **stepping number** is an integer such that all of its adjacent digits have an absolute difference of **exactly** `1`.

Return *an integer denoting the count of stepping numbers in the inclusive range* `[low, high]`*.*

Since the answer may be very large, return it **modulo** `109 + 7`.

**Note:** A stepping number should not have a leading zero.

**Example 1:**

**Input:** low = "1", high = "11"
**Output:** 10
**Explanation:** The stepping numbers in the range \[1,11\] are 1, 2, 3, 4, 5, 6, 7, 8, 9 and 10. There are a total of 10 stepping numbers in the range. Hence, the output is 10.

**Example 2:**

**Input:** low = "90", high = "101"
**Output:** 2
**Explanation:** The stepping numbers in the range \[90,101\] are 98 and 101. There are a total of 2 stepping numbers in the range. Hence, the output is 2. 

**Constraints:**

-   `1 <= int(low) <= int(high) < 10100`
-   `1 <= low.length, high.length <= 100`
-   `low` and `high` consist of only digits.
-   `low` and `high` don't have any leading zeros.

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
const int N = 105;  
const int MOD = 1e9 + 7;  
  
int s[N];  
unordered_map<int, int> memo;  
  
int init = []() {  
    int f[10];  
    f[0] = 0;  
    fill(f + 1, f + 10, 1);  
    s[1] = 9;  
    for (int i = 2; i < N; i++) {  
        s[i] += s[i - 1];  
        int g[10];  
        memset(g, 0, sizeof g);  
        for (int j = 0; j <= 9; j++) {  
            if (j) g[j] = f[j - 1];  
            if (j < 9) g[j] = (g[j] + f[j + 1]) % MOD;  
            s[i] = (s[i] + g[j]) % MOD;  
        }  
        memcpy(f, g, sizeof g);  
    }  
    return 0;  
}();  
  
int helper(int start, int n) {  
    if (!n) return 1;  
    int key = n * 10 + start;  
    if (memo.contains(key)) {  
        return memo[key];  
    }  
    int f[10];  
    memset(f, 0, sizeof f);  
    f[start] = 1;  
    for (int i = 1; i <= n; i++) {  
        int g[10];  
        memset(g, 0, sizeof g);  
        for (int j = 0; j <= 9; j++) {  
            if (j) g[j] = f[j - 1];  
            if (j < 9) g[j] = (g[j] + f[j + 1]) % MOD;  
        }  
        memcpy(f, g, sizeof g);  
    }  
    int res = 0;  
    for (int j = 0; j <= 9; j++) {  
        res = (res + f[j]) % MOD;  
    }  
    memo[key] = res;  
    return res;  
}  
  
int get(string &limit) {  
    auto n = limit.size();  
    int res = n > 1 ? s[n - 1] : 0;  
    for (int i = 1, prev = -1; i <= n; i++) {  
        int d = limit[i - 1] - '0';  
        for (int j = i == 1; j < d; j++) {  
            if (i > 1 && abs(j - prev) != 1) continue;  
            res = (res + helper(j, n - i)) % MOD;  
        }  
        if (i > 1 && abs(d - prev) != 1) break;  
        if (i == n) res = (res + 1) % MOD;  
        prev = d;  
    }  
    return res;  
}  
  
class Solution {  
public:  
    int countSteppingNumbers(string low, string high) {  
        int res = (get(high) - get(low) + MOD) % MOD;  
        int t = 1;  
        for (int i = 1; i < low.size(); i++) {  
            if (abs(low[i] - low[i - 1]) != 1) {  
                t = 0;  
                break;  
            }  
        }  
        res = (res + t) % MOD;  
        return res;  
    }  
};
```