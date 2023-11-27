---
page-title: "2944. 购买水果需要的最少金币数 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/minimum-number-of-coins-for-fruits/
date: "2023-11-27 15:48:42"
---
You are at a fruit market with different types of exotic fruits on display.

You are given a **1-indexed** array `prices`, where `prices[i]` denotes the number of coins needed to purchase the `ith` fruit.

The fruit market has the following offer:

-   If you purchase the `ith` fruit at `prices[i]` coins, you can get the next `i` fruits for free.

**Note** that even if you **can** take fruit `j` for free, you can still purchase it for `prices[j]` coins to receive a new offer.

Return *the **minimum** number of coins needed to acquire all the fruits*.

**Example 1:**

**Input:** prices = \[3,1,2\]
**Output:** 4
**Explanation:** You can acquire the fruits as follows:
- Purchase the 1st fruit with 3 coins, you are allowed to take the 2nd fruit for free.
- Purchase the 2nd fruit with 1 coin, you are allowed to take the 3rd fruit for free.
- Take the 3rd fruit for free.
Note that even though you were allowed to take the 2nd fruit for free, you purchased it because it is more optimal.
It can be proven that 4 is the minimum number of coins needed to acquire all the fruits.

**Example 2:**

**Input:** prices = \[1,10,1,1\]
**Output:** 2
**Explanation:** You can acquire the fruits as follows:
- Purchase the 1st fruit with 1 coin, you are allowed to take the 2nd fruit for free.
- Take the 2nd fruit for free.
- Purchase the 3rd fruit for 1 coin, you are allowed to take the 4th fruit for free.
- Take the 4th fruit for free.
It can be proven that 2 is the minimum number of coins needed to acquire all the fruits.

**Constraints:**

-   `1 <= prices.length <= 1000`
-   `1 <= prices[i] <= 105`

DP，O($N^2$)
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int minimumCoins(vector<int>&prices) {
        int n = prices.size();
        int f[n + 1];
        memset(f, 0x3f, sizeof f);
        for (int i = n; i; i--) {
            if (i >= (n + 1) / 2) f[i] = prices[i - 1];
            else {
                for (int j = i + 1; j <= i * 2 + 1; j++) {
                    f[i] = min(f[i], f[j] + prices[i - 1]);
                }
            }
        }
        return f[1];
    }
};
```

单调队列优化, O($N$)
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int minimumCoins(vector<int>&prices) {
        int n = prices.size();
        deque<pair<int, int>> q;
        q.emplace_back(n + 1, 0);
        for (int i = n; i; i--) {
            while (q.back().first > i * 2 + 1) q.pop_back();
            int f = prices[i - 1] + q.back().second;
            while (f <= q.front().second) q.pop_front();
            q.emplace_front(i, f);
        }
        return q.front().second;
    }
};
```