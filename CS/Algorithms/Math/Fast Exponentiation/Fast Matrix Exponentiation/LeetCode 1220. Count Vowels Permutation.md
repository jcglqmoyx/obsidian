---
page-title: "Count Vowels Permutation - LeetCode"
url: https://leetcode.com/problems/count-vowels-permutation/description/
date: "2023-11-02 18:08:49"
---
Given an integer `n`, your task is to count how many strings of length `n` can be formed under the following rules:

-   Each character is a lower case vowel (`'a'`, `'e'`, `'i'`, `'o'`, `'u'`)
-   Each vowel `'a'` may only be followed by an `'e'`.
-   Each vowel `'e'` may only be followed by an `'a'` or an `'i'`.
-   Each vowel `'i'` **may not** be followed by another `'i'`.
-   Each vowel `'o'` may only be followed by an `'i'` or a `'u'`.
-   Each vowel `'u'` may only be followed by an `'a'`.

Since the answer may be too large, return it modulo `10^9 + 7`.

**Example 1:**

**Input:** n = 1
**Output:** 5
**Explanation:** All possible strings are: "a", "e", "i" , "o" and "u".

**Example 2:**

**Input:** n = 2
**Output:** 10
**Explanation:** All possible strings are: "ae", "ea", "ei", "ia", "ie", "io", "iu", "oi", "ou" and "ua".

**Example 3:** 

**Input:** n = 5
**Output:** 68

**Constraints:**

-   `1 <= n <= 2 * 10^4`

DP
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int countVowelPermutation(int n) {
        vector<vector<int>> s(5);
        int mod = 1e9 + 7;
        s[0] = {1, 2, 4};
        s[1] = {0, 2};
        s[2] = {1, 3};
        s[3] = {2};
        s[4] = {2, 3};
        vector<int> f(5, 1);
        for (int it = 2; it <= n; it++) {
            vector<int> g(5);
            for (int i = 0; i < 5; i++) {
                for (int idx: s[i]) {
                    g[i] = (g[i] + f[idx]) % mod;
                }
            }
            f = std::move(g);
        }
        int res = 0;
        for (int i = 0; i < 5; i++) {
            res = (res + f[i]) % mod;
        }
        return res;
    }
};
```

Fast exponentiation
```cpp
#include <bits/stdc++.h>

using namespace std;

using LL = long long;

const int MOD = 1e9 + 7;

vector<vector<LL>> times(vector<vector<LL>> &a, vector<vector<LL>> &b) {
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

vector<vector<LL>> quick_power(vector<vector<LL>> &a, int n) {
    vector<vector<LL>> res = a;
    n--;
    while (n) {
        if (n & 1) res = times(res, a);
        a = times(a, a);
        n >>= 1;
    }
    return res;
}

class Solution {
public:
    int countVowelPermutation(int n) {
        if (n == 1) return 5;
        vector<vector<LL>> matrix = {
                {0, 1, 0, 0, 0},
                {1, 0, 1, 0, 0},
                {1, 1, 0, 1, 1},
                {0, 0, 1, 0, 1},
                {1, 0, 0, 0, 0}
        };
        vector<vector<LL>> f1(1, vector<LL>({1LL, 1LL, 1LL, 1LL, 1LL}));
        auto expo = quick_power(matrix, n - 1);
        auto fn = times(f1, expo);
        return (int) (accumulate(fn[0].begin(), fn[0].end(), 0LL) % MOD);
    }
};
```