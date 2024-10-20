---
page-title: LeetCode 3325. Count Substrings With K-Frequency Characters I
url: https://leetcode.cn/problems/count-substrings-with-k-frequency-characters-i/description/
date: 2024-10-20 15:23:56
---
Given a string `s` and an integer `k`, return the total number of substring of `s` where **at least one** character appears **at least** `k` times.

**Example 1:**

**Input:** s = "abacb", k = 2

**Output:** 4

**Explanation:**

The valid substrings are:

-   `"aba"` (character `'a'` appears 2 times).
-   `"abac"` (character `'a'` appears 2 times).
-   `"abacb"` (character `'a'` appears 2 times).
-   `"bacb"` (character `'b'` appears 2 times).

**Example 2:**

**Input:** s = "abcde", k = 1

**Output:** 15

**Explanation:**

All substrings are valid because every character appears at least once.

**Constraints:**

-   `1 <= s.length <= 3000`
-   `1 <= k <= s.length`
-   `s` consists only of lowercase English letters.

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int numberOfSubstrings(string s, int k) {  
        unordered_map<int, int> cnt;  
        int res = 0;  
        for (int l = 0, r = 0; r < s.size(); r++) {  
            cnt[s[r]]++;  
            while (cnt[s[r]] >= k) {  
                cnt[s[l]]--;  
                l++;  
            }  
            res += l;  
        }  
        return res;  
    }  
};
```