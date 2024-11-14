---
page-title: LeetCode 3352. Count K-Reducible Numbers Less Than N
url: https://leetcode.cn/problems/count-k-reducible-numbers-less-than-n/description/
date: 2024-11-14 19:14:32
---
You are given a **binary** string `s` representing a number `n` in its binary form.

You are also given an integer `k`.

An integer `x` is called **k-reducible** if performing the following operation **at most** `k` times reduces it to 1:

-   Replace `x` with the **count** of in its binary representation.

For example, the binary representation of 6 is `"110"`. Applying the operation once reduces it to 2 (since `"110"` has two set bits). Applying the operation again to 2 (binary `"10"`) reduces it to 1 (since `"10"` has one set bit).

Return an integer denoting the number of positive integers **less** than `n` that are **k-reducible**.

Since the answer may be too large, return it **modulo** `109 + 7`.

**Example 1:**

**Input:** s = "111", k = 1

**Output:** 3

**Explanation:**

`n = 7`. The 1-reducible integers less than 7 are 1, 2, and 4.

**Example 2:**

**Input:** s = "1000", k = 2

**Output:** 6

**Explanation:**

`n = 8`. The 2-reducible integers less than 8 are 1, 2, 3, 4, 5, and 6.

**Example 3:**

**Input:** s = "1", k = 3

**Output:** 0

**Explanation:**

There are no positive integers less than `n = 1`, so the answer is 0.

**Constraints:**

-   `1 <= s.length <= 800`
-   `s` has no leading zeros.
-   `s` consists only of the characters `'0'` and `'1'`.
-   `1 <= k <= 5`

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
const int MOD = 1e9 + 7;  
const int N = 805;  
  
int op[N];  
int comb[N][N];  
  
int init = []() {  
    for (int i = 1; i < N; i++) {  
        int x = i;  
        while (x != 1) {  
            x = __builtin_popcount(x);  
            op[i]++;  
            if (op[x]) {  
                op[i] += op[x];  
                break;  
            }  
        }  
    }  
  
    for (int i = 0; i < N; i++) {  
        comb[i][0] = 1;  
        for (int j = 1; j <= i; j++) {  
            comb[i][j] = (comb[i - 1][j] + comb[i - 1][j - 1]) % MOD;  
        }  
    }  
    return 0;  
}();  
  
class Solution {  
public:  
    int countKReducibleNumbers(string s, int k) {  
        int n = static_cast<int>(s.size());  
        if (n == 1) return 0;  
        int res = 0;  
        for (int i = 1; i < n; i++) {  
            if (op[i] + 1 <= k) {  
                res = (res + comb[n - 1][i]) % MOD;  
            }  
        }  
        for (int cnt = s[0] == '1', i = 1; i < n; i++) {  
            if (s[i] == '1') {  
                for (int j = 0; j <= n - i - 1; j++) {  
                    if (op[cnt + j] + 1 <= k) {  
                        res = (res + comb[n - i - 1][j]) % MOD;  
                    }  
                }  
                cnt++;  
            }  
        }  
        return res;  
    }  
};
```