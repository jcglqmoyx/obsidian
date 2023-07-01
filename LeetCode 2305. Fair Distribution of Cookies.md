---
page-title: "2305. 公平分发饼干 - 力扣（Leetcode）"
url: https://leetcode.cn/problems/fair-distribution-of-cookies/description/
date: "2023-07-01 12:42:12"
---
You are given an integer array `cookies`, where `cookies[i]` denotes the number of cookies in the `ith` bag. You are also given an integer `k` that denotes the number of children to distribute **all** the bags of cookies to. All the cookies in the same bag must go to the same child and cannot be split up.

The **unfairness** of a distribution is defined as the **maximum** **total** cookies obtained by a single child in the distribution.

Return *the **minimum** unfairness of all distributions*.

**Example 1:**

**Input:** cookies = \[8,15,10,20,8\], k = 2
**Output:** 31
**Explanation:** One optimal distribution is \[8,15,8\] and \[10,20\]
- The 1st child receives \[8,15,8\] which has a total of 8 + 15 + 8 = 31 cookies.
- The 2nd child receives \[10,20\] which has a total of 10 + 20 = 30 cookies.
The unfairness of the distribution is max(31,30) = 31.
It can be shown that there is no distribution with an unfairness less than 31.

**Example 2:**

**Input:** cookies = \[6,1,3,2,2,4,1,2\], k = 3
**Output:** 7
**Explanation:** One optimal distribution is \[6,1\], \[3,2,2\], and \[4,1,2\]
- The 1st child receives \[6,1\] which has a total of 6 + 1 = 7 cookies.
- The 2nd child receives \[3,2,2\] which has a total of 3 + 2 + 2 = 7 cookies.
- The 3rd child receives \[4,1,2\] which has a total of 4 + 1 + 2 = 7 cookies.
The unfairness of the distribution is max(7,7,7) = 7.
It can be shown that there is no distribution with an unfairness less than 7.

**Constraints:**

-   `2 <= cookies.length <= 8`
-   `1 <= cookies[i] <= 105`
-   `2 <= k <= cookies.length`
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    /**
     * state-compression dp
     * time O(k⋅3^n), where n is the length of cookies
     */
    int distributeCookies(vector<int> &cookies, int k) {
        int n = (int) cookies.size();
        int f[1 << n], s[1 << n];
        memset(f, 0x3f, sizeof f);
        for (int i = 1; i < 1 << n; i++) {
            int sum = 0;
            for (int j = 0; j < n; j++) {
                if (i >> j & 1) {
                    sum += cookies[j];
                }
            }
            f[i] = s[i] = sum;
        }
        for (int i = 1; i < k; i++) {
            for (int j = (1 << n) - 1; j; j--) {
                for (int sub = j; sub; sub = (sub - 1) & j) {
                    f[j] = min(f[j], max(f[j - sub], s[sub]));
                }
            }
        }
        return f[(1 << n) - 1];
    }
};
```