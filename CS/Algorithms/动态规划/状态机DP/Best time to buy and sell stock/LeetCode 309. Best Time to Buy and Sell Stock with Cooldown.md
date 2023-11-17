---
page-title: "309. 买卖股票的最佳时机含冷冻期 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/
date: "2023-10-03 10:09:32"
---

> [309\. Best Time to Buy and Sell Stock with Cooldown](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

---

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

-   After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example 1:**

**Input:** prices = \[1,2,3,0,2\]
**Output:** 3
**Explanation:** transactions = \[buy, sell, cooldown, buy, sell\]

**Example 2:**

**Input:** prices = \[1\]
**Output:** 0

**Constraints:**

-   `1 <= prices.length <= 5000`
-   `0 <= prices[i] <= 1000`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int maxProfit(vector<int> &prices) {
        int n = (int) prices.size();
        /**
         * f[i][0]: has stock
         * f[i][1]: don't have stock, in cooldown
         * f[i][2]: don't have stock, not in cooldown
         */
        int f[n][3];
        memset(f, -0x3f, sizeof f);
        f[0][0] = -prices[0], f[0][1] = 0, f[0][2] = 0;
        for (int i = 1; i < n; i++) {
            f[i][0] = max(f[i - 1][0], f[i - 1][2] - prices[i]);
            f[i][1] = f[i - 1][0] + prices[i];
            f[i][2] = max(f[i - 1][1], f[i - 1][2]);
        }
        return max(f[n - 1][1], f[n - 1][2]);
    }
};
```