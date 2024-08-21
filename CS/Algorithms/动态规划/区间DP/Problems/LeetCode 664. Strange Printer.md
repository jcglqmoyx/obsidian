---
page-title: LeetCode 664. Strange Printer
url: https://leetcode.com/problems/strange-printer/description/?envType=daily-question&envId=2024-08-21
date: 2024-08-21 13:41:40
---
There is a strange printer with the following two special properties:

-   The printer can only print a sequence of **the same character** each time.
-   At each turn, the printer can print new characters starting from and ending at any place and will cover the original existing characters.

Given a string `s`, return *the minimum number of turns the printer needed to print it*.

**Example 1:**

**Input:** s = "aaabbb"
**Output:** 2
**Explanation:** Print "aaa" first and then print "bbb".

**Example 2:**

**Input:** s = "aba"
**Output:** 2
**Explanation:** Print "aaa" first and then print "b" from the second place of the string, which will cover the existing character 'a'.

**Constraints:**

-   `1 <= s.length <= 100`
-   `s` consists of lowercase English letters.

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int strangePrinter(string s) {
        int n = s.size();
        int f[n + 1][n + 1];
        for (int i = n - 1; i >= 0; i--) {
            f[i][i] = 1;
            for (int j = i + 1; j < n; j++) {
                f[i][i] = 1;
                if (s[i] == s[j]) {
                    f[i][j] = f[i][j - 1];
                } else {
                    int mn = INT32_MAX;
                    for (int k = i; k < j; k++) {
                        mn = min(mn, f[i][k] + f[k + 1][j]);
                    }
                    f[i][j] = mn;
                }
            }
        }
        return f[0][n - 1];
    }
};
```