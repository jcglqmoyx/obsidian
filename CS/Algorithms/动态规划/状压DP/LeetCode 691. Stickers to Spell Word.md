---
page-title: LeetCode 691. Stickers to Spell Word
url: https://leetcode.com/problems/stickers-to-spell-word/description/
date: 2024-09-15 00:32:44
---
We are given `n` different types of `stickers`. Each sticker has a lowercase English word on it.

You would like to spell out the given string `target` by cutting individual letters from your collection of stickers and rearranging them. You can use each sticker more than once if you want, and you have infinite quantities of each sticker.

Return *the minimum number of stickers that you need to spell out* `target`. If the task is impossible, return `-1`.

**Note:** In all test cases, all words were chosen randomly from the `1000` most common US English words, and `target` was chosen as a concatenation of two random words.

**Example 1:**

**Input:** stickers = \["with","example","science"\], target = "thehat"
**Output:** 3
**Explanation:**
We can use 2 "with" stickers, and 1 "example" sticker.
After cutting and rearrange the letters of those stickers, we can form the target "thehat".
Also, this is the minimum number of stickers necessary to form the target string.

**Example 2:**

**Input:** stickers = \["notice","possible"\], target = "basicbasic"
**Output:** -1
Explanation:
We cannot form the target "basicbasic" from cutting letters from the given stickers.

**Constraints:**

-   `n == stickers.length`
-   `1 <= n <= 50`
-   `1 <= stickers[i].length <= 10`
-   `1 <= target.length <= 15`
-   `stickers[i]` and `target` consist of lowercase English letters.

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int minStickers(vector<string> &stickers, string target) {
        auto n = stickers.size(), m = target.size();
        int cs[n][26];
        memset(cs, 0, sizeof cs);
        for (int i = 0; i < n; i++) {
            for (char c: stickers[i]) {
                cs[i][c - 'a']++;
            }
        }
        int f[1 << m], inf = 0x3f3f3f3f;
        memset(f, -1, sizeof f);
        function<int(int)> dp = [&](int mask) -> int {
            if (!mask) return 0;
            if (f[mask] != -1) return f[mask];
            f[mask] = inf;
            for (int i = 0; i < n; i++) {
                int new_mask = mask;
                int dt[26]{};
                for (int j = 0; j < m; j++) {
                    if (mask >> j & 1) {
                        int u = target[j] - 'a';
                        if (dt[u] < cs[i][u]) {
                            dt[u]++;
                            new_mask ^= 1 << j;
                        }
                    }
                }
                if (new_mask != mask) {
                    f[mask] = min(f[mask], dp(new_mask) + 1);
                }
            }
            return f[mask];
        };
        int res = dp((1 << m) - 1);
        if (res >= inf) res = -1;
        return res;
    }
};
```