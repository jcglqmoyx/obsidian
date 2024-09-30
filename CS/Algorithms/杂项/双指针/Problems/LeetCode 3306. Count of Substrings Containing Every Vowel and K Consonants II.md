---
page-title: LeetCode 3306. Count of Substrings Containing Every Vowel and K Consonants II
url: https://leetcode.com/problems/count-of-substrings-containing-every-vowel-and-k-consonants-ii/description/
date: 2024-09-29 15:38:09
---
You are given a string `word` and a **non-negative** integer `k`.

Return the total number of substrings of `word` that contain every vowel (`'a'`, `'e'`, `'i'`, `'o'`, and `'u'`) **at least** once and **exactly** `k` consonants.

**Example 1:**

**Input:** word = "aeioqq", k = 1

**Output:** 0

**Explanation:**

There is no substring with every vowel.

**Example 2:**

**Input:** word = "aeiou", k = 0

**Output:** 1

**Explanation:**

The only substring with every vowel and zero consonants is `word[0..4]`, which is `"aeiou"`.

**Example 3:**

**Input:** word = "ieaouqqieaouqq", k = 1

**Output:** 3

**Explanation:**

The substrings with every vowel and one consonant are:

-   `word[0..5]`, which is `"ieaouq"`.
-   `word[6..11]`, which is `"qieaou"`.
-   `word[7..12]`, which is `"ieaouq"`.

**Constraints:**

-   `5 <= word.length <= 2 * 105`
-   `word` consists only of lowercase English letters.
-   `0 <= k <= word.length - 5`

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
int get(char c) {  
    if (c == 'a') return 0;  
    if (c == 'e') return 1;  
    if (c == 'i') return 2;  
    if (c == 'o') return 3;  
    if (c == 'u') return 4;  
    return 5;  
}  
  
long long calc(string &s, int k) {  
    long long n = s.size(), res = 0;  
    unordered_map<int, int> cnt;  
    int consonant = 0;  
    for (int l = 0, r = 0; r < n; r++) {  
        int idx = get(s[r]);  
        if (idx < 5) cnt[idx]++;  
        else consonant++;  
        while (l <= r && consonant >= k && cnt.size() == 5) {  
            int u = get(s[l]);  
            if (u < 5) {  
                if (--cnt[u] == 0) cnt.erase(u);  
            } else {  
                consonant--;  
            }  
            l++;  
        }  
        res += l;  
    }  
    return res;  
}  
  
class Solution {  
public:  
    long long countOfSubstrings(string word, int k) {  
        return calc(word, k) - calc(word, k + 1);  
    }  
};
```