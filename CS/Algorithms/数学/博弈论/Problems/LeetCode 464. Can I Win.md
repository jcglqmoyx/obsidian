---
page-title: LeetCode 464. Can I Win
url: https://leetcode.com/problems/can-i-win/description/
date: 2024-10-10 13:38:19
---
In the "100 game" two players take turns adding, to a running total, any integer from `1` to `10`. The player who first causes the running total to **reach or exceed** 100 wins.

What if we change the game so that players **cannot** re-use integers?

For example, two players might take turns drawing from a common pool of numbers from 1 to 15 without replacement until they reach a total >= 100.

Given two integers `maxChoosableInteger` and `desiredTotal`, return `true` if the first player to move can force a win, otherwise, return `false`. Assume both players play **optimally**.

**Example 1:**

**Input:** maxChoosableInteger = 10, desiredTotal = 11
**Output:** false
**Explanation:**
No matter which integer the first player choose, the first player will lose.
The first player can choose an integer from 1 up to 10.
If the first player choose 1, the second player can only choose integers from 2 up to 10.
The second player will win by choosing 10 and get a total = 11, which is >= desiredTotal.
Same with other integers chosen by the first player, the second player will always win.

**Example 2:**

**Input:** maxChoosableInteger = 10, desiredTotal = 0
**Output:** true

**Example 3:**

**Input:** maxChoosableInteger = 10, desiredTotal = 1
**Output:** true

**Constraints:**

-   `1 <= maxChoosableInteger <= 20`
-   `0 <= desiredTotal <= 300`

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    bool canIWin(int maxChoosableInteger, int desiredTotal) {  
        if ((1 + maxChoosableInteger) * maxChoosableInteger / 2 < desiredTotal) return false;  
        int f[1 << maxChoosableInteger];  
        memset(f, -1, sizeof f);  
        auto dp = [&](auto &&dp, int mask, int s) -> bool {  
            if (f[mask] != -1) return f[mask];  
            for (int i = 0; i < maxChoosableInteger; i++) {  
                if (mask >> i & 1) continue;  
                if (s + i + 1 >= desiredTotal || !dp(dp, mask | 1 << i, s + i + 1)) return f[mask] = true;  
            }  
            return f[mask] = false;  
        };  
        return dp(dp, 0, 0);  
    }  
};
```