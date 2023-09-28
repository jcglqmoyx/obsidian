## [LeetCode 1016. Binary String With Substrings Representing 1 To N](https://leetcode.cn/problems/binary-string-with-substrings-representing-1-to-n/)
---
Given a binary string `s` and a positive integer `n`, return `true` *if the binary representation of all the integers in the range* `[1, n]` *are **substrings** of* `s`*, or* `false` *otherwise*.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** s = "0110", n = 3
**Output:** true

**Example 2:**

**Input:** s = "0110", n = 4
**Output:** false

**Constraints:**

-   `1 <= s.length <= 1000`
-   `s[i]` is either `'0'` or `'1'`.
-   `1 <= n <= 1e9`
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    bool help(const string &s, int k, int mi, int ma) {
        unordered_set<int> st;
        int t = 0;
        for (int r = 0; r < s.size(); ++r) {
            t = t * 2 + (s[r] - '0');
            if (r >= k) t -= (s[r - k] - '0') << k;
            if (r >= k - 1 && t >= mi && t <= ma) st.insert(t);
        }
        return st.size() == ma - mi + 1;
    }

    bool queryString(string s, int n) {
        if (n == 1) return s.find('1') != -1;
        int k = 30;
        while ((1 << k) >= n) k--;
        if (s.size() < (1 << (k - 1)) + k - 1 || s.size() < n - (1 << k) + k + 1) return false;
        return help(s, k, 1 << (k - 1), (1 << k) - 1) && help(s, k + 1, 1 << k, n);
    }
};
```