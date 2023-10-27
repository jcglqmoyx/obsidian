---
page-title: "5. 最长回文子串 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/longest-palindromic-substring/?envType=daily-question&envId=2023-10-27
date: "2023-10-27 11:04:11"
---
Given a string `s`, return *the longest palindromic* *substring* in `s`.

**Example 1:**

**Input:** s = "babad"
**Output:** "bab"
**Explanation:** "aba" is also a valid answer.

**Example 2:**

**Input:** s = "cbbd"
**Output:** "bb"

**Constraints:**

-   `1 <= s.length <= 1000`
-   `s` consist of only digits and English letters.

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    string longestPalindrome(string s) {
        string t = "$";
        for (char c: s) {
            t.push_back('#');
            t.push_back(c);
        }
        t += "#^";
        int n = (int) t.size();
        int p[n];
        memset(p, 0, sizeof p);
        int mr = 0, mid = 0;
        for (int i = 1; i < n - 1; i++) {
            if (i < mr) p[i] = min(p[2 * mid - i], mr - i);
            else p[i] = 1;
            while (t[i + p[i]] == t[i - p[i]]) p[i]++;
            if (p[i] + i > mr) {
                mr = p[i] + i;
                mid = i;
            }
        }
        int idx = 0, mx = 0;
        for (int i = 1; i < n - 1; i++) {
            if (p[i] > mx) {
                mx = p[i];
                idx = (i - p[i]) / 2;
            }
        }
        return s.substr(idx, mx - 1);
    }
};
```