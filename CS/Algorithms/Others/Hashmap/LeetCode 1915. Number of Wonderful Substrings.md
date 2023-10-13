---
page-title: "1915. 最美子字符串的数目 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/number-of-wonderful-substrings/description/
date: "2023-10-13 10:34:42"
---
A **wonderful** string is a string where **at most one** letter appears an **odd** number of times.

-   For example, `"ccjjc"` and `"abab"` are wonderful, but `"ab"` is not.

Given a string `word` that consists of the first ten lowercase English letters (`'a'` through `'j'`), return *the **number of wonderful non-empty substrings** in* `word`*. If the same substring appears multiple times in* `word`*, then count **each occurrence** separately.*

A **substring** is a contiguous sequence of characters in a string.

**Example 1:**

**Input:** word = "aba"
**Output:** 4
**Explanation:** The four wonderful substrings are underlined below:
- "**a**ba" -> "a"
- "a**b**a" -> "b"
- "ab**a**" -> "a"
- "**aba**" -> "aba"

**Example 2:**

**Input:** word = "aabb"
**Output:** 9
**Explanation:** The nine wonderful substrings are underlined below:
- "**a**abb" -> "a"
- "**aa**bb" -> "aa"
- "**aab**b" -> "aab"
- "**aabb**" -> "aabb"
- "a**a**bb" -> "a"
- "a**abb**" -> "abb"
- "aa**b**b" -> "b"
- "aa**bb**" -> "bb"
- "aab**b**" -> "b"

**Example 3:**

**Input:** word = "he"
**Output:** 2
**Explanation:** The two wonderful substrings are underlined below:
- "**h**e" -> "h"
- "h**e**" -> "e"

**Constraints:**

-   `1 <= word.length <= 105`
-   `word` consists of lowercase English letters from `'a'` to `'j'`.

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    long long wonderfulSubstrings(string word) {
        long long res = 0;
        unordered_map<int, int> cnt;
        cnt[0] = 1;
        int mask = 0;
        for (char c: word) {
            mask ^= 1 << (c - 'a');
            res += cnt[mask];
            cnt[mask]++;
            for (int i = 0; i < 10; i++) {
                res += cnt[mask ^ (1 << i)];
            }
        }
        return res;
    }
};
```