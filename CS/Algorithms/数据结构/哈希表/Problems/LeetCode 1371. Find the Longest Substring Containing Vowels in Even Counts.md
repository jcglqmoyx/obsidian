---
page-title: "1371. 每个元音包含偶数次的最长子字符串 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/find-the-longest-substring-containing-vowels-in-even-counts/description/
date: "2023-10-09 10:20:54"
---
Given the string `s`, return the size of the longest substring containing each vowel an even number of times. That is, 'a', 'e', 'i', 'o', and 'u' must appear an even number of times.

**Example 1:**

**Input:** s = "eleetminicoworoep"
**Output:** 13
**Explanation:** The longest substring is "leetminicowor" which contains two each of the vowels: **e**, **i** and **o** and zero of the vowels: **a** and **u**.

**Example 2:**

**Input:** s = "leetcodeisgreat"
**Output:** 5
**Explanation:** The longest substring is "leetc" which contains two e's.

**Example 3:**

**Input:** s = "bcbcbc"
**Output:** 6
**Explanation:** In this case, the given string "bcbcbc" is the longest because all vowels: **a**, **e**, **i**, **o** and **u** appear zero times.

**Constraints:**

-   `1 <= s.length <= 5 x 10^5`
-   `s` contains only lowercase English letters.

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int findTheLongestSubstring(string &s) {
        int p[33], status = 0, ans = 0;
        memset(p, -1, sizeof p);
        p[0] = 0;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == 'a') status ^= 1;
            else if (s[i] == 'e') status ^= 1 << 1;
            else if (s[i] == 'i') status ^= 1 << 2;
            else if (s[i] == 'o') status ^= 1 << 3;
            else if (s[i] == 'u') status ^= 1 << 4;
            if (p[status] == -1) p[status] = i + 1;
            else ans = max(i - p[status] + 1, ans);
        }
        return ans;
    }
};
```