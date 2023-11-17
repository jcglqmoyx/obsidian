## [LeetCode 1434. Number of Ways to Wear Different Hats to Each Other](https://leetcode.cn/problems/number-of-ways-to-wear-different-hats-to-each-other/description/)
There are `n` people and `40` types of hats labeled from `1` to `40`.

Given a 2D integer array `hats`, where `hats[i]` is a list of all hats preferred by the `ith` person.

Return _the number of ways that the `n` people wear different hats to each other_.

Since the answer may be too large, return it modulo `109 + 7`.

**Example 1:**

**Input:** hats = [[3,4],[4,5],[5]]
**Output:** 1
**Explanation:** There is only one way to choose hats given the conditions. 
First person choose hat 3, Second person choose hat 4 and last one hat 5.

**Example 2:**

**Input:** hats = [[3,5,1],[3,5]]
**Output:** 4
**Explanation:** There are 4 ways to choose hats:
(3,5), (5,3), (1,3) and (1,5)

**Example 3:**

**Input:** hats = [[1,2,3,4],[1,2,3,4],[1,2,3,4],[1,2,3,4]]
**Output:** 24
**Explanation:** Each person can choose hats labeled from 1 to 4.
Number of Permutations of (1,2,3,4) = 24.

**Constraints:**

-   `n == hats.length`
-   `1 <= n <= 10`
-   `1 <= hats[i].length <= 40`
-   `1 <= hats[i][j] <= 40`
-   `hats[i]` contains a list of **unique** integers.

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int numberWays(vector<vector<int>> &hats) {  
        const int MOD = 1e9 + 7;  
        int n = (int) hats.size();  
  
        vector<long long> memo(n);  
        int max_id = 0;  
        for (int i = 0; i < n; i++) {  
            long long mask = 0;  
            for (int hat: hats[i]) {  
                mask |= 1LL << hat;  
                max_id = max(max_id, hat);  
            }  
            memo[i] = mask;  
        }  
  
        int f[max_id + 1][1 << n];  
        memset(f, 0, sizeof f);  
        f[0][0] = 1;  
        for (int i = 1; i <= max_id; i++) {  
            for (int j = 0; j < 1 << n; j++) {  
                f[i][j] = f[i - 1][j];  
                for (int k = 0; k < n; k++) {  
                    if (j >> k & 1 && memo[k] >> i & 1) {  
                        f[i][j] += f[i - 1][j - (1 << k)];  
                        f[i][j] %= MOD;  
                    }  
                }  
            }  
        }  
        return f[max_id][(1 << n) - 1];  
    }  
};
```