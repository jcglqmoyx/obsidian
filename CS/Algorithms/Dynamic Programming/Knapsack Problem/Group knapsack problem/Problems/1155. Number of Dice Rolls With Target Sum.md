---
page-title: "1155. 掷骰子等于目标和的方法数 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/number-of-dice-rolls-with-target-sum/description/?envType=daily-question&envId=2023-10-24
date: "2023-10-24 09:19:26"
---
You have `n` dice, and each die has `k` faces numbered from `1` to `k`.

Given three integers `n`, `k`, and `target`, return *the number of possible ways (out of the* `kn` *total ways)* *to roll the dice, so the sum of the face-up numbers equals* `target`. Since the answer may be too large, return it **modulo** `109 + 7`.

**Example 1:**

**Input:** n = 1, k = 6, target = 3
**Output:** 1
**Explanation:** You throw one die with 6 faces.
There is only one way to get a sum of 3.

**Example 2:**

**Input:** n = 2, k = 6, target = 7
**Output:** 6
**Explanation:** You throw two dice, each with 6 faces.
There are 6 ways to get a sum of 7: 1+6, 2+5, 3+4, 4+3, 5+2, 6+1.

**Example 3:**

**Input:** n = 30, k = 30, target = 500
**Output:** 222616187
**Explanation:** The answer must be returned modulo 109 + 7.

**Constraints:**

-   `1 <= n, k <= 30`
-   `1 <= target <= 1000`

O(n ⋅ k ⋅ target)
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int numRollsToTarget(int n, int k, int target) {
        int f[target + 1], g[target + 1], MOD = 1e9 + 7;
        memset(f, 0, sizeof f);
        f[0] = 1;
        for (int it = 0; it < n; it++) {
            memset(g, 0, sizeof g);
            for (int i = 1; i <= k; i++) {
                for (int j = target; j >= i; j--) {
                    g[j] = (g[j] + f[j - i]) % MOD;
                }
            }
            memcpy(f, g, sizeof g);
        }
        return f[target];
    }
};
```

O(n ⋅ k ⋅ (target - n))
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int numRollsToTarget(int n, int k, int target) {
        if (target < n || target > n * k) {
            return 0;
        }
        int f[target - n + 1], g[target - n + 1], MOD = 1e9 + 7;
        memset(f, 0, sizeof f);
        f[0] = 1;
        for (int it = 0; it < n; it++) {
            memset(g, 0, sizeof g);
            for (int i = 0; i < k; i++) {
                for (int j = target - n; j >= i; j--) {
                    g[j] = (g[j] + f[j - i]) % MOD;
                }
            }
            memcpy(f, g, sizeof g);
        }
        return f[target - n];
    }
};
```

O(n⋅(target−n))
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int numRollsToTarget(int n, int k, int target) {
        if (target < n || target > n * k) {
            return 0;
        }
        int f[target - n + 1], MOD = 1e9 + 7;
        memset(f, 0, sizeof f);
        f[0] = 1;
        for (int i = 1; i <= n; i++) {
            int mx = min(i * (k - 1), target - n);
            for (int j = 1; j <= mx; j++) {
                f[j] = (f[j] + f[j - 1]) % MOD;
            }
            for (int j = mx; j >= k; j--) {
                f[j] = (f[j] - f[j - k] + MOD) % MOD;
            }
        }
        return f[target - n];
    }
};
```