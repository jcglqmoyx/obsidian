---
page-title: "Sum of Prefix Scores of Strings - LeetCode"
url: https://leetcode.com/problems/sum-of-prefix-scores-of-strings/description/
date: "2023-11-25 16:39:50"
---
You are given an array `words` of size `n` consisting of **non-empty** strings.

We define the **score** of a string `word` as the **number** of strings `words[i]` such that `word` is a **prefix** of `words[i]`.

-   For example, if `words = ["a", "ab", "abc", "cab"]`, then the score of `"ab"` is `2`, since `"ab"` is a prefix of both `"ab"` and `"abc"`.

Return *an array* `answer` *of size* `n` *where* `answer[i]` *is the **sum** of scores of every **non-empty** prefix of* `words[i]`.

**Note** that a string is considered as a prefix of itself.

**Example 1:**

**Input:** words = \["abc","ab","bc","b"\]
**Output:** \[5,4,3,2\]
**Explanation:** The answer for each string is the following:
- "abc" has 3 prefixes: "a", "ab", and "abc".
- There are 2 strings with the prefix "a", 2 strings with the prefix "ab", and 1 string with the prefix "abc".
The total is answer\[0\] = 2 + 2 + 1 = 5.
- "ab" has 2 prefixes: "a" and "ab".
- There are 2 strings with the prefix "a", and 2 strings with the prefix "ab".
The total is answer\[1\] = 2 + 2 = 4.
- "bc" has 2 prefixes: "b" and "bc".
- There are 2 strings with the prefix "b", and 1 string with the prefix "bc".
The total is answer\[2\] = 2 + 1 = 3.
- "b" has 1 prefix: "b".
- There are 2 strings with the prefix "b".
The total is answer\[3\] = 2.

**Example 2:**

**Input:** words = \["abcd"\]
**Output:** \[4\]
**Explanation:**
"abcd" has 4 prefixes: "a", "ab", "abc", and "abcd".
Each prefix has a score of one, so the total is answer\[0\] = 1 + 1 + 1 + 1 = 4.

**Constraints:**

-   `1 <= words.length <= 1000`
-   `1 <= words[i].length <= 1000`
-   `words[i]` consists of lowercase English letters.


```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1000001;

int son[N][26], idx;
int cnt[N];

void insert(string&s) {
    int p = 0;
    for (char&c: s) {
        int u = c - 'a';
        if (!son[p][u]) son[p][u] = ++idx;
        p = son[p][u];
        cnt[p]++;
    }
}

int query(string&s) {
    int res = 0, p = 0;
    for (char&c: s) {
        int u = c - 'a';
        p = son[p][u];
        res += cnt[p];
    }
    return res;
}

class Solution {
public:
    vector<int> sumPrefixScores(vector<string>&words) {
        memset(son, 0, sizeof son), idx = 0;
        memset(cnt, 0, sizeof cnt);
        for (auto&s: words) insert(s);
        int n = (int)words.size();
        vector<int> res(n);
        for (int i = 0; i < n; i++) {
            res[i] = query(words[i]);
        }
        return res;
    }
};
```
