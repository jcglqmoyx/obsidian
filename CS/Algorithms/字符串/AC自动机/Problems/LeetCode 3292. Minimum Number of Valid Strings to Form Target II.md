---
page-title: LeetCode 3292. Minimum Number of Valid Strings to Form Target II
url: https://leetcode.cn/problems/minimum-number-of-valid-strings-to-form-target-ii/description/
date: 2024-09-16 10:05:37
---
You are given an array of strings `words` and a string `target`.

A string `x` is called **valid** if `x` is a prefix of **any** string in `words`.

Return the **minimum** number of **valid** strings that can be *concatenated* to form `target`. If it is **not** possible to form `target`, return `-1`.

A prefix of a string is a substring that starts from the beginning of the string and extends to any point within it.

**Example 1:**

**Input:** words = \["abc","aaaaa","bcdef"\], target = "aabcdabc"

**Output:** 3

**Explanation:**

The target string can be formed by concatenating:

-   Prefix of length 2 of `words[1]`, i.e. `"aa"`.
-   Prefix of length 3 of `words[2]`, i.e. `"bcd"`.
-   Prefix of length 3 of `words[0]`, i.e. `"abc"`.

**Example 2:**

**Input:** words = \["abababab","ab"\], target = "ababaababa"

**Output:** 2

**Explanation:**

The target string can be formed by concatenating:

-   Prefix of length 5 of `words[0]`, i.e. `"ababa"`.
-   Prefix of length 5 of `words[0]`, i.e. `"ababa"`.

**Example 3:**

**Input:** words = \["abcdef"\], target = "xyz"

**Output:** \-1

**Constraints:**

-   `1 <= words.length <= 100`
-   `1 <= words[i].length <= 5 * 104`
-   The input is generated such that `sum(words[i].length) <= 105`.
-   `words[i]` consists only of lowercase English letters.
-   `1 <= target.length <= 5 * 104`
-   `target` consists only of lowercase English letters.

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 100010, M = 50010;

int tr[N][26], idx;
int ne[N], len[N];
int q[N];
int f[M];

void insert(string &s) {
    int p = 0;
    for (int i = 0; i < s.size(); i++) {
        int u = s[i] - 'a';
        if (!tr[p][u]) tr[p][u] = ++idx;
        p = tr[p][u];
        len[p] = i + 1;
    }
}

void build() {
    int hh = 0, tt = -1;
    for (int i = 0; i < 26; i++) {
        if (tr[0][i]) {
            q[++tt] = tr[0][i];
        }
    }
    while (hh <= tt) {
        int t = q[hh++];
        for (int i = 0; i < 26; i++) {
            int p = tr[t][i];
            if (!p) tr[t][i] = tr[ne[t]][i];
            else {
                ne[p] = tr[ne[t]][i];
                q[++tt] = p;
            }
        }
    }
}

class Solution {
public:
    int minValidStrings(vector<string> &words, string target) {
        int tot = 1;
        for (auto &w: words) {
            tot += (int) w.size();
        }
        for (int i = 0; i < tot; i++) {
            memset(tr[i], 0, sizeof tr[i]);
        }
        idx = 0;
        memset(ne, 0, sizeof(int) * tot);
        memset(len, 0, sizeof(int) * tot);
        for (auto &w: words) {
            insert(w);
        }
        build();
        auto n = target.size();
        memset(f, 0, sizeof(int) * n);
        for (int i = 0, p = 0; i < n; i++) {
            int t = target[i] - 'a';
            p = tr[p][t];
            int l = len[p];
            if (l) {
                f[i - l + 1] = max(f[i - l + 1], l);
            }
        }
        int res = 0;
        for (int end = 0, mx = 0, i = 0; i < n; i++) {
            mx = max(mx, i + f[i]);
            if (mx > n) break;
            if (mx == i) return -1;
            if (i == end) end = mx, res++;
        }
        return res;
    }
};
```