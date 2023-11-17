## [LeetCode 2561. Rearranging Fruits](https://leetcode.cn/problems/rearranging-fruits/description/)

You have two fruit baskets containing `n` fruits each. You are given two **0-indexed** integer arrays `basket1` and `basket2` representing the cost of fruit in each basket. You want to make both baskets **equal**. To do so, you can use the following operation as many times as you want:

-   Chose two indices `i` and `j`, and swap the `i$^{th}$` fruit of `basket1` with the `j$^{th}$`fruit of `basket2`.
-   The cost of the swap is `min(basket1[i],basket2[j])`.

Two baskets are considered equal if sorting them according to the fruit cost makes them exactly the same baskets.

Return _the minimum cost to make both the baskets equal or_ `-1` _if impossible._

**Example 1:**

```
Input: basket1 = [4,2,2,2], basket2 = [1,4,1,2]
Output: 1
Explanation: Swap index 1 of basket1 with index 0 of basket2, which has cost 1. Now basket1 = [4,1,2,2] and basket2 = [2,4,1,2]. Rearranging both the arrays makes them equal.
```

**Example 2:**

```
Input: basket1 = [2,3,4,1], basket2 = [3,2,5,1]
Output: -1
Explanation: It can be shown that it is impossible to make both the baskets equal.
```

**Constraints:**

-   `basket1.length == bakste2.length`
-   `1 <= basket1.length <= $10^5$`
-   `1 <= basket1[i],basket2[i] <= $10^9$`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    long long minCost(vector<int> &basket1, vector<int> &basket2) {
        unordered_map<int, int> cnt;
        for (int i = 0; i < basket1.size(); i++) {
            cnt[basket1[i]]++;
            cnt[basket2[i]]--;
        }
        int mn = INT32_MAX;
        vector<int> v;
        for (auto &[k, c]: cnt) {
            if (c & 1) return -1;
            mn = min(mn, k);
            c = abs(c) / 2;
            while (c--) v.push_back(k);
        }
        nth_element(v.begin(), v.begin() + (int) v.size() / 2, v.end());
        long long res = 0;
        for (int i = 0; i < v.size() / 2; i++) {
            res += min(mn * 2, v[i]);
        }
        return res;
    }
};
```