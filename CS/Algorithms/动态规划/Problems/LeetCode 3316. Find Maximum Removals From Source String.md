---
page-title: LeetCode 3316. Find Maximum Removals From Source String
url: https://leetcode.cn/problems/find-maximum-removals-from-source-string/description/
date: 2024-10-14 22:19:04
---
You are given a string `source` of size `n`, a string `pattern` that is a subsequence

of `source`, and a **sorted** integer array `targetIndices` that contains **distinct** numbers in the range `[0, n - 1]`.

We define an **operation** as removing a character at an index `idx` from `source` such that:

-   `idx` is an element of `targetIndices`.
-   `pattern` remains a subsequence of `source` after removing the character.

Performing an operation **does not** change the indices of the other characters in `source`. For example, if you remove `'c'` from `"acb"`, the character at index 2 would still be `'b'`.

Return the **maximum** number of *operations* that can be performed.

**Example 1:**

**Input:** source = "abbaa", pattern = "aba", targetIndices = \[0,1,2\]

**Output:** 1

**Explanation:**

We can't remove `source[0]` but we can do either of these two operations:

-   Remove `source[1]`, so that `source` becomes `"a_baa"`.
-   Remove `source[2]`, so that `source` becomes `"ab_aa"`.

**Example 2:**

**Input:** source = "bcda", pattern = "d", targetIndices = \[0,3\]

**Output:** 2

**Explanation:**

We can remove `source[0]` and `source[3]` in two operations.

**Example 3:**

**Input:** source = "dda", pattern = "dda", targetIndices = \[0,1,2\]

**Output:** 0

**Explanation:**

We can't remove any character from `source`.

**Example 4:**

**Input:** source = "yeyeykyded", pattern = "yeyyd", targetIndices = \[0,2,3,4\]

**Output:** 2

**Explanation:**

We can remove `source[2]` and `source[3]` in two operations.

**Constraints:**

-   `1 <= n == source.length <= 3 * 103`
-   `1 <= pattern.length <= n`
-   `1 <= targetIndices.length <= n`
-   `targetIndices` is sorted in ascending order.
-   The input is generated such that `targetIndices` contains distinct elements in the range `[0, n - 1]`.
-   `source` and `pattern` consist only of lowercase English letters.
-   The input is generated such that `pattern` appears as a subsequence in `source`.

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int maxRemovals(string source, string pattern, vector<int> &targetIndices) {  
        int n = static_cast<int>(source.size()), m = static_cast<int>(pattern.size());  
        bool st[n + 1];  
        memset(st, 0, sizeof st);  
        for (int x: targetIndices) {  
            st[x + 1] = true;  
        }  
        int f[m + 1];  
        memset(f, -0x3f, sizeof f);  
        f[0] = 0;  
        for (int i = 1; i <= n; i++) {  
            for (int j = min(i, m); j >= 1; j--) {  
                f[j] += st[i];  
                if (source[i - 1] == pattern[j - 1]) {  
                    f[j] = max(f[j], f[j - 1]);  
                }  
            }  
            f[0] += st[i];  
        }  
        return f[m];  
    }  
};
```