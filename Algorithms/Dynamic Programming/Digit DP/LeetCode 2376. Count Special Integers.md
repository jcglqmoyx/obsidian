---
page-title: "Count Special Integers - LeetCode"
url: https://leetcode.com/problems/count-special-integers/description/
date: "2023-07-08 19:38:15"
---

We call a positive integer **special** if all of its digits are **distinct**.

Given a **positive** integer `n`, return *the number of special integers that belong to the interval* `[1, n]`.

**Example 1:**

**Input:** n = 20
**Output:** 19
**Explanation:** All the integers from 1 to 20, except 11, are special. Thus, there are 19 special integers.

**Example 2:**

**Input:** n = 5
**Output:** 5
**Explanation:** All the integers from 1 to 5 are special.

**Example 3:**

**Input:** n = 135
**Output:** 110
**Explanation:** There are 110 integers from 1 to 135 that are special.
Some of the integers that are not special are: 22, 114, and 131.

**Constraints:**

-   `1 <= n <= 2 * 109`

### iteration
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int countSpecialNumbers(int n) {
        vector<int> digits;
        while (n) digits.push_back(n % 10), n /= 10;
        reverse(digits.begin(), digits.end());
        int res = 0;
        for (int len = 1; len < digits.size(); len++) {
            int t = 9;
            for (int i = 9, j = 1; j < len; i--, j++) {
                t *= i;
            }
            res += t;
        }
        bool st[10] = {};
        for (int i = 0; i < digits.size(); i++) {
            for (int j = !i; j < digits[i]; j++) {
                if (st[j]) continue;
                int t = 1;
                for (int k = i + 1; k < digits.size(); k++) {
                    t *= 10 - k;
                }
                res += t;
            }
            if (st[digits[i]]) break;
            st[digits[i]] = true;
        }
        unordered_set<int> S(digits.begin(), digits.end());
        if (S.size() == digits.size()) res++;
        return res;
    }
};
```

### dfs
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int countSpecialNumbers(int n) {
        int m = n;
        vector<int> digits;
        while (m) digits.push_back(m % 10), m /= 10;
        reverse(digits.begin(), digits.end());
        m = (int) digits.size();
        int f[m][10][1 << 10];
        memset(f, -1, sizeof f);
        function<int(int, int, int, bool)> dp = [&](int u, int d, int mask, bool limit) -> int {
            if (u == m) return 1;
            if (!limit && f[u][d][mask] != -1) return f[u][d][mask];
            int res = 0, mx = limit ? digits[u] : 9;
            for (int i = u == 0 ? 1 : 0; i <= mx; i++) {
                if ((mask | (1 << i)) == mask) continue;
                res += dp(u + 1, i, mask | (1 << i), limit && i == mx);
            }
            if (!limit) f[u][d][mask] = res;
            return res;
        };
        int res = dp(0, 0, 0, true);
        for (int i = 1; i < m; i++) {
            int t = 9;
            for (int j = 1, op = 9; j < i; j++, op--) {
                t *= op;
            }
            res += t;
        }
        return res;
    }
};
```