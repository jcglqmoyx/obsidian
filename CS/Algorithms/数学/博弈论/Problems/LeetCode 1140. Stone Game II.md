---
page-title: LeetCode 1140. Stone Game II
url: https://leetcode.cn/problems/stone-game-ii/description/?envType=daily-question&envId=2024-08-20
date: 2024-08-20 15:34:10
---
Alice and Bob continue their games with piles of stones.  There are a number of piles **arranged in a row**, and each pile has a positive integer number of stones `piles[i]`.  The objective of the game is to end with the most stones. 

Alice and Bob take turns, with Alice starting first.  Initially, `M = 1`.

On each player's turn, that player can take **all the stones** in the **first** `X` remaining piles, where `1 <= X <= 2M`.  Then, we set `M = max(M, X)`.

The game continues until all the stones have been taken.

Assuming Alice and Bob play optimally, return the maximum number of stones Alice can get.

**Example 1:**

**Input:** piles = \[2,7,9,4,4\]
**Output:** 10
**Explanation:**  If Alice takes one pile at the beginning, Bob takes two piles, then Alice takes 2 piles again. Alice can get 2 + 4 + 4 = 10 piles in total. If Alice takes two piles at the beginning, then Bob can take all three piles left. In this case, Alice get 2 + 7 = 9 piles in total. So we return 10 since it's larger. 

**Example 2:**

**Input:** piles = \[1,2,3,4,5,100\]
**Output:** 104

**Constraints:**

-   `1 <= piles.length <= 100`
-   `1 <= piles[i] <= 104`


```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int stoneGameII(vector<int> &piles) {
        auto n = piles.size();
        int f[n + 2][n + 1], s[n + 1];
        memset(f, 0, sizeof f);
        s[0] = 0;
        for (int i = 1; i <= n; i++) {
            s[i] = s[i - 1] + piles[i - 1];
        }
        for (int i = n; i; i--) {
            for (int j = 1; j <= n; j++) {
                for (int k = 1; i + k - 1 <= n && k <= j * 2; k++) {
                    f[i][j] = max(f[i][j], s[n] - s[i - 1] - f[i + k][max(k, j)]);
                }
            }
        }
        return f[1][1];
    }
};
```