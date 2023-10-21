---
page-title: "316. 去除重复字母 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/remove-duplicate-letters/description/?envType=daily-question&envId=2023-09-26
date: "2023-09-26 10:55:07"
---

> [316\. Remove Duplicate Letters](https://leetcode.cn/problems/remove-duplicate-letters/)

---

Given a string `s`, remove duplicate letters so that every letter appears once and only once. You must make sure your result is

**the smallest in lexicographical order**

among all possible results.

**Example 1:**

**Input:** s = "bcabc"
**Output:** "abc"

**Example 2:**

**Input:** s = "cbacdcbc"
**Output:** "acdb"

**Constraints:**

-   `1 <= s.length <= 104`
-   `s` consists of lowercase English letters.

**Note:** This question is the same as 1081: [https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/](https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/)
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    string removeDuplicateLetters(string s) {
        int cnt[26]{};
        for (char c: s) cnt[c - 'a']++;
        int vis = 0;
        string t;
        for (char c: s) {
            int u = c - 'a';
            if (!(vis >> u & 1)) {
                while (!t.empty() && t.back() > c && cnt[t.back() - 'a']) {
                    int v = t.back() - 'a';
                    vis ^= 1 << v;
                    t.pop_back();
                }
                t.push_back(c);
                vis |= 1 << u;
            }
            cnt[u]--;
        }
        return t;
    }
};
```