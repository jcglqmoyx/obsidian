---
page-title: "1542. 找出最长的超赞子字符串 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/find-longest-awesome-substring/description/
date: "2023-10-10 08:44:52"
---
You are given a string `s`. An **awesome** substring is a non-empty substring of `s` such that we can make any number of swaps in order to make it a palindrome.

Return *the length of the maximum length **awesome substring** of* `s`.

**Example 1:**

**Input:** s = "3242415"
**Output:** 5
**Explanation:** "24241" is the longest awesome substring, we can form the palindrome "24142" with some swaps.

**Example 2:**

**Input:** s = "12345678"
**Output:** 1

**Example 3:**

**Input:** s = "213123"
**Output:** 6
**Explanation:** "213123" is the longest awesome substring, we can form the palindrome "231132" with some swaps.

**Constraints:**

-   `1 <= s.length <= 105`
-   `s` consists only of digits.

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int longestAwesome(string s) {
        int p[1 << 10], res = 0;
        memset(p, 0x3f, sizeof p);
        p[0] = -1;
        for (int mask = 0, i = 0; i < s.size(); i++) {
            int x = s[i] - '0';
            mask ^= 1 << x;
            res = max(res, i - p[mask]);
            p[mask] = min(p[mask], i);
            for (int j = 0; j < 10; j++) {
                res = max(res, i - p[mask ^ (1 << j)]);
            }
        }
        return res;
    }
};
```