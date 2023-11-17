---
page-title: "String Transformation - LeetCode"
url: https://leetcode.com/problems/string-transformation/description/
date: "2023-11-02 19:57:54"
---
You are given two strings `s` and `t` of equal length `n`. You can perform the following operation on the string `s`:

-   Remove a **suffix** of `s` of length `l` where `0 < l < n` and append it at the start of `s`.  
    For example, let `s = 'abcd'` then in one operation you can remove the suffix `'cd'` and append it in front of `s` making `s = 'cdab'`.

You are also given an integer `k`. Return *the number of ways in which* `s` *can be transformed into* `t` *in **exactly*** `k` *operations.*

Since the answer can be large, return it **modulo** `109 + 7`.

**Example 1:**

**Input:** s = "abcd", t = "cdab", k = 2
**Output:** 2
**Explanation:** 
First way:
In first operation, choose suffix from index = 3, so resulting s = "dabc".
In second operation, choose suffix from index = 3, so resulting s = "cdab".

Second way:
In first operation, choose suffix from index = 1, so resulting s = "bcda".
In second operation, choose suffix from index = 1, so resulting s = "cdab".

**Example 2:**

**Input:** s = "ababab", t = "ababab", k = 1
**Output:** 2
**Explanation:** 
First way:
Choose suffix from index = 2, so resulting s = "ababab".

Second way:
Choose suffix from index = 4, so resulting s = "ababab".

**Constraints:**

-   `2 <= s.length <= 5 * 105`
-   `1 <= k <= 1015`
-   `s.length == t.length`
-   `s` and `t` consist of only lowercase English alphabets.

DP, O(s.length() + k), time limit exceeded

```cpp
#include <bits/stdc++.h>

using namespace std;

using LL = long long;

const int MOD = 1e9 + 7;

class Solution {
public:
    int numberOfWays(string s, string t, long long k) {
        int n = (int) s.size();
        int ne[n];
        memset(ne, -1, sizeof ne);
        int cnt = 0;
        for (int i = 1, j = -1; i < n; i++) {
            while (j != -1 && t[j + 1] != t[i]) j = ne[j];
            if (t[j + 1] == t[i]) j++;
            ne[i] = j;
        }
        for (int i = 1, j = 0; i < n + n - 1; i++) {
            while (j != -1 && t[j + 1] != s[i % n]) j = ne[j];
            if (t[j + 1] == s[i % n]) j++;
            if (j == n - 1) cnt++, j = ne[j];
        }

        if (cnt == 0) {
            return 0;
        }

        LL f[k + 1][2];
//        f[i][0]: i operations, s becomes t
//        f[i][1]: i operations, s doesn't become t
        if (s == t) {
            f[1][0] = cnt - 1;
            f[1][1] = n - cnt;
        } else {
            f[1][0] = cnt;
            f[1][1] = n - cnt - 1;
        }
        for (int i = 2; i <= k; i++) {
            f[i][0] = (f[i - 1][0] * (cnt - 1) + f[i - 1][1] * cnt) % MOD;
            f[i][1] = (f[i - 1][0] * (n - cnt) + f[i - 1][1] * (n - cnt - 1)) % MOD;
        }
        return (int) f[k][0];
    }
};
```


Using fast exponentiation to optimize, O(s.length() + log k), accepted
```cpp
#include <bits/stdc++.h>

using namespace std;

using LL = long long;

const int MOD = 1e9 + 7;

vector<vector<LL>> multiply(vector<vector<LL>> &a, vector<vector<LL>> &b) {
    int n1 = (int) a.size(), m1 = (int) a[0].size(), m2 = (int) b[0].size();
    vector<vector<LL>> res(n1, vector<LL>(m2));
    for (int i = 0; i < n1; i++) {
        for (int j = 0; j < m2; j++) {
            for (int k = 0; k < m1; k++) {
                res[i][j] = (res[i][j] + a[i][k] * b[k][j]) % MOD;
            }
        }
    }
    return res;
}

vector<vector<LL>> quick_power(vector<vector<LL>> &mat, LL n) {
    vector<vector<LL>> res = mat;
    n--;
    while (n) {
        if (n & 1) res = multiply(res, mat);
        mat = multiply(mat, mat);
        n >>= 1;
    }
    return res;
}

class Solution {
public:
    int numberOfWays(string s, string t, long long k) {
        int n = (int) s.size();
        int ne[n];
        memset(ne, -1, sizeof ne);
        int cnt = 0;
        for (int i = 1, j = -1; i < n; i++) {
            while (j != -1 && t[j + 1] != t[i]) j = ne[j];
            if (t[j + 1] == t[i]) j++;
            ne[i] = j;
        }
        for (int i = 1, j = 0; i < n + n - 1; i++) {
            while (j != -1 && t[j + 1] != s[i % n]) j = ne[j];
            if (t[j + 1] == s[i % n]) j++;
            if (j == n - 1) cnt++, j = ne[j];
        }

        vector<vector<LL>> mat = {{cnt - 1, n - cnt},
                                  {cnt,     n - cnt - 1}};
        int f_1_0, f_1_1;
        if (s == t) f_1_0 = cnt - 1, f_1_1 = n - cnt;
        else f_1_0 = cnt, f_1_1 = n - cnt - 1;
        if (k == 1) return f_1_0;
        auto p = quick_power(mat, k - 1);
        return (int) ((f_1_0 * p[0][0] + f_1_1 * p[1][0]) % MOD);
    }
};
```
