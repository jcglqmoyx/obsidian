---
page-title: LeetCode 2707. Extra Characters in a String
url: https://leetcode.cn/problems/extra-characters-in-a-string/description/?envType=daily-question&envId=2024-09-23
date: 2024-09-23 13:36:52
---
You are given a **0-indexed** string `s` and a dictionary of words `dictionary`. You have to break `s` into one or more **non-overlapping** substrings such that each substring is present in `dictionary`. There may be some **extra characters** in `s` which are not present in any of the substrings.

Return *the **minimum** number of extra characters left over if you break up* `s` *optimally.*

**Example 1:**

**Input:** s = "leetscode", dictionary = \["leet","code","leetcode"\]
**Output:** 1
**Explanation:** We can break s in two substrings: "leet" from index 0 to 3 and "code" from index 5 to 8. There is only 1 unused character (at index 4), so we return 1.

**Example 2:**

**Input:** s = "sayhelloworld", dictionary = \["hello","world"\]
**Output:** 3
**Explanation:** We can break s in two substrings: "hello" from index 3 to 7 and "world" from index 8 to 12. The characters at indices 0, 1, 2 are not used in any substring and thus are considered as extra characters. Hence, we return 3.

**Constraints:**

-   `1 <= s.length <= 50`
-   `1 <= dictionary.length <= 50`
-   `1 <= dictionary[i].length <= 50`
-   `dictionary[i]` and `s` consists of only lowercase English letters
-   `dictionary` contains distinct words

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int minExtraChar(string s, vector<string> &dictionary) {  
        auto n = s.size();  
        int tot = 1;  
        for (auto &s: dictionary) tot += s.size();  
        int tr[tot][26], idx = 0;  
        memset(tr, 0, sizeof tr);  
        bool is_end[tot];  
        memset(is_end, 0, sizeof is_end);  
  
        for (auto &w: dictionary) {  
            int p = 0;  
            for (int i = w.size() - 1; i >= 0; i--) {  
                int u = w[i] - 'a';  
                if (!tr[p][u]) tr[p][u] = ++idx;  
                p = tr[p][u];  
            }  
            is_end[p] = true;  
        }  
  
        int f[n + 1];  
        f[0] = 0;  
  
        for (int i = 1; i <= n; i++) {  
            f[i] = f[i - 1] + 1;  
            int p = 0;  
            for (int j = i; j; j--) {  
                int u = s[j - 1] - 'a';  
                if (!tr[p][u]) break;  
                p = tr[p][u];  
                if (is_end[p]) f[i] = min(f[i], f[j - 1]);  
            }  
        }  
        return f[n];  
    }  
};
```