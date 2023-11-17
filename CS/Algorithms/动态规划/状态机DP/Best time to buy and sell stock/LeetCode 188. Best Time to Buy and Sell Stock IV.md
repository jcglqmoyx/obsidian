---
page-title: "188. 买卖股票的最佳时机 IV - 力扣（LeetCode）"
url: https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/
date: "2023-10-03 09:25:38"
---

> [188\. Best Time to Buy and Sell Stock IV](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/)

---

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day, and an integer `k`.

Find the maximum profit you can achieve. You may complete at most `k` transactions: i.e. you may buy at most `k` times and sell at most `k` times.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example 1:**

**Input:** k = 2, prices = \[2,4,1\]
**Output:** 2
**Explanation:** Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.

**Example 2:**

**Input:** k = 2, prices = \[3,2,6,5,0,3\]
**Output:** 7
**Explanation:** Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6-2 = 4. Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.

**Constraints:**

-   `1 <= k <= 100`
-   `1 <= prices.length <= 1000`
-   `0 <= prices[i] <= 1000`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int maxProfit(int k, vector<int> &prices) {
        k = min(k, (int) prices.size() / 2) + 1;
        vector<int> buy(k, -1e9), sell(k, 0);
        for (int i: prices) {
            for (int j = 1; j < k; j++) {
                buy[j] = max(buy[j], sell[j - 1] - i);
                sell[j] = max(sell[j], buy[j] + i);
            }
        }
        return sell.back();
    }
};
```