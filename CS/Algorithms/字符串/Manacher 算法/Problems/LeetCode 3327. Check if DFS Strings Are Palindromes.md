---
page-title: LeetCode 3327. Check if DFS Strings Are Palindromes
url: https://leetcode.cn/problems/check-if-dfs-strings-are-palindromes/description/
date: 2024-10-20 16:14:36
---
You are given a tree rooted at node 0, consisting of `n` nodes numbered from `0` to `n - 1`. The tree is represented by an array `parent` of size `n`, where `parent[i]` is the parent of node `i`. Since node 0 is the root, `parent[0] == -1`.

You are also given a string `s` of length `n`, where `s[i]` is the character assigned to node `i`.

Consider an empty string `dfsStr`, and define a recursive function `dfs(int x)` that takes a node `x` as a parameter and performs the following steps in order:

-   Iterate over each child `y` of `x` **in increasing order of their numbers**, and call `dfs(y)`.
-   Add the character `s[x]` to the end of the string `dfsStr`.

**Note** that `dfsStr` is shared across all recursive calls of `dfs`.

You need to find a boolean array `answer` of size `n`, where for each index `i` from `0` to `n - 1`, you do the following:

-   Empty the string `dfsStr` and call `dfs(i)`.
-   If the resulting string `dfsStr` is a **palindrome**, then set `answer[i]` to `true`. Otherwise, set `answer[i]` to `false`.

Return the array `answer`.

A **palindrome** is a string that reads the same forward and backward.

**Example 1:**

![](https://assets.leetcode.com/uploads/2024/09/01/tree1drawio.png)

**Input:** parent = \[-1,0,0,1,1,2\], s = "aababa"

**Output:** \[true,true,false,true,true,true\]

**Explanation:**

-   Calling `dfs(0)` results in the string `dfsStr = "abaaba"`, which is a palindrome.
-   Calling `dfs(1)` results in the string `dfsStr = "aba"`, which is a palindrome.
-   Calling `dfs(2)` results in the string `dfsStr = "ab"`, which is **not** a palindrome.
-   Calling `dfs(3)` results in the string `dfsStr = "a"`, which is a palindrome.
-   Calling `dfs(4)` results in the string `dfsStr = "b"`, which is a palindrome.
-   Calling `dfs(5)` results in the string `dfsStr = "a"`, which is a palindrome.

**Example 2:**

![](https://assets.leetcode.com/uploads/2024/09/01/tree2drawio-1.png)

**Input:** parent = \[-1,0,0,0,0\], s = "aabcb"

**Output:** \[true,true,true,true,true\]

**Explanation:**

Every call on `dfs(x)` results in a palindrome string.

**Constraints:**

-   `n == parent.length == s.length`
-   `1 <= n <= 105`
-   `0 <= parent[i] <= n - 1` for all `i >= 1`.
-   `parent[0] == -1`
-   `parent` represents a valid tree.
-   `s` consists only of lowercase English letters.

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    vector<bool> findAnswer(vector<int> &parent, string s) {  
        int n = static_cast<int>(parent.size());  
        int h[n], e[n], ne[n], idx = 0;  
        memset(h, -1, sizeof h);  
        for (int i = n - 1; i; i--) {  
            // 因为链式前向星是头插法，所以这里先枚举一个node的序号较大的子节点  
            e[idx] = i, ne[idx] = h[parent[i]], h[parent[i]] = idx++;  
        }  
        string str;  
        int start[n], end[n], time = 0;  
        auto dfs = [&](auto &&dfs, int u) -> void {  
            start[u] = time;  
            for (int i = h[u]; ~i; i = ne[i]) {  
                dfs(dfs, e[i]);  
            }  
            str.push_back(s[u]);  
            end[u] = time++;  
        };  
  
        dfs(dfs, 0);  
  
        s = "^#";  
        for (char c: str) {  
            s.push_back(c);  
            s.push_back('#');  
        }  
        s.push_back('$');  
        int m = static_cast<int>(s.size());  
        int d[m + 1];  
        memset(d, 0, sizeof d);  
        d[1] = 1;  
        for (int i = 2, l = 1, r = 1; i < m; i++) {  
            if (i <= r) d[i] = min(d[l + r - i], r - i + 1);  
            while (s[i - d[i]] == s[i + d[i]]) d[i]++;  
            if (i + d[i] - 1 > r) l = i - d[i] + 1, r = i + d[i] - 1;  
        }  
  
        vector<bool> res(n);  
        for (int i = 0; i < n; i++) {  
            int l = (start[i] + 1) * 2;  
            int r = (end[i] + 1) * 2;  
            res[i] = d[(l + r) >> 1] >= (r - l) / 2 + 1;  
        }  
        return res;  
    }  
};
```