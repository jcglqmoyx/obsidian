---
page-title: "2434. 使用机器人打印字典序最小的字符串 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/using-a-robot-to-print-the-lexicographically-smallest-string/description/
date: "2023-10-07 14:00:36"
---
You are given a string `s` and a robot that currently holds an empty string `t`. Apply one of the following operations until `s` and `t` **are both empty**:

-   Remove the **first** character of a string `s` and give it to the robot. The robot will append this character to the string `t`.
-   Remove the **last** character of a string `t` and give it to the robot. The robot will write this character on paper.

Return *the lexicographically smallest string that can be written on the paper.*

**Example 1:**

**Input:** s = "zza"
**Output:** "azz"
**Explanation:** Let p denote the written string.
Initially p="", s="zza", t="".
Perform first operation three times p="", s="", t="zza".
Perform second operation three times p="azz", s="", t="".

**Example 2:**

**Input:** s = "bac"
**Output:** "abc"
**Explanation:** Let p denote the written string.
Perform first operation twice p="", s="c", t="ba". 
Perform second operation twice p="ab", s="c", t="". 
Perform first operation p="ab", s="", t="c". 
Perform second operation p="abc", s="", t="".

**Example 3:**

**Input:** s = "bdda"
**Output:** "addb"
**Explanation:** Let p denote the written string.
Initially p="", s="bdda", t="".
Perform first operation four times p="", s="", t="bdda".
Perform second operation four times p="addb", s="", t="".

**Constraints:**

-   `1 <= s.length <= 105`
-   `s` consists of only English lowercase letters.

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    string robotWithString(string s) {
        int n = (int) s.size();
        char f[n + 1];
        f[n] = 'z' + 1;
        for (int i = n - 1; ~i; i--) {
            f[i] = min(s[i], f[i + 1]);
        }
        stack<char> stk;
        string res;
        for (int i = 0; i < n; i++) {
            stk.emplace(s[i]);
            while (!stk.empty() && stk.top() <= f[i + 1]) {
                res.push_back(stk.top());
                stk.pop();
            }
        }
        return res;
    }
};
```