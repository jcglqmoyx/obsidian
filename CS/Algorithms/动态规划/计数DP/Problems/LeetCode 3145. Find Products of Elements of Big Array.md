---
page-title: LeetCode 3145. Find Products of Elements of Big Array
url: https://leetcode.cn/problems/find-products-of-elements-of-big-array/description/?envType=daily-question&envId=2024-08-23
date: 2024-08-23 19:40:36
---
The **powerful array** of a non-negative integer `x` is defined as the shortest sorted array of powers of two that sum up to `x`. The table below illustrates examples of how the **powerful array** is determined. It can be proven that the powerful array of `x` is unique.

| num | Binary Representation | powerful array |
| --- | --- | --- |
| 1 | 00001 | \[1\] |
| 8 | 01000 | \[8\] |
| 10 | 01010 | \[2, 8\] |
| 13 | 01101 | \[1, 4, 8\] |
| 23 | 10111 | \[1, 2, 4, 16\] |

The array `big_nums` is created by concatenating the **powerful arrays** for every positive integer `i` in ascending order: 1, 2, 3, and so on. Thus, `big_nums` begins as `[1, 2, 1, 2, 4, 1, 4, 2, 4, 1, 2, 4, 8, ...]`.

You are given a 2D integer matrix `queries`, where for `queries[i] = [fromi, toi, modi]` you should calculate `(big_nums[fromi] * big_nums[fromi + 1] * ... * big_nums[toi]) % modi`.

Return an integer array `answer` such that `answer[i]` is the answer to the `ith` query.

**Example 1:**

**Input:** queries = \[\[1,3,7\]\]

**Output:** \[4\]

**Explanation:**

There is one query.

`big_nums[1..3] = [2,1,2]`. The product of them is 4. The result is `4 % 7 = 4.`

**Example 2:**

**Input:** queries = \[\[2,5,3\],\[7,7,4\]\]

**Output:** \[2,2\]

**Explanation:**

There are two queries.

First query: `big_nums[2..5] = [1,2,4,1]`. The product of them is 8. The result is `8 % 3 = 2`.

Second query: `big_nums[7] = 2`. The result is `2 % 4 = 2`.

**Constraints:**

-   `1 <= queries.length <= 500`
-   `queries[i].length == 3`
-   `0 <= queries[i][0] <= queries[i][1] <= 1015`
-   `1 <= queries[i][2] <= 105`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
    using ll = long long;

    int power(ll x, ll n, int mod) {
        ll res = 1;
        while (n) {
            if (n & 1) res = res * x % mod;
            x = x * x % mod;
            n >>= 1;
        }
        return res;
    }

    // the number of bits in the range[1, x]
    vector<ll> get(ll x) {
        ll temp = x;
        vector<int> v;
        while (temp) {
            v.emplace_back(temp & 1);
            temp >>= 1;
        }
        int n = v.size();
        vector<ll> res(50);
        if (x == 0) return res;
        reverse(v.begin(), v.end());
        ll left = 0;
        ll right[n + 1];
        right[n] = 0;
        for (int i = n - 1; i > 0; i--) {
            right[i] = v[i] * (1LL << (n - i - 1)) + right[i + 1];
        }
        for (int i = 0; i < n; i++) {
            int u = x >> (n - i - 1) & 1;
            res[n - i - 1] += left * (1LL << (n - i - 1));
            if (u) {
                res[n - i - 1] += right[i + 1] + 1;
            }
            left = left * 2 + v[i];
        }
        return res;
    }

public:
    vector<int> findProductsOfElements(vector<vector<long long>> &queries) {
        auto m = queries.size();
        vector<int> res(m);
        for (int i = 0; i < m; i++) {
            ll from = queries[i][0], to = queries[i][1] + 1, mod = queries[i][2];
            ll l = 0, r = to;
            while (l < r) {
                ll mid = (l + r + 1) >> 1;
                auto t = get(mid);
                if (accumulate(t.begin(), t.end(), 0LL) <= to) l = mid;
                else r = mid - 1;
            }
            auto t = get(l);
            ll tot = accumulate(t.begin(), t.end(), 0LL);
            for (int u = 0; u < 50 && to > tot; u++) {
                if ((l + 1) >> u & 1) {
                    t[u]++;
                    to--;
                }
            }
            l = 0, r = from;
            while (l < r) {
                ll mid = (l + r + 1) >> 1;
                auto t = get(mid);
                if (accumulate(t.begin(), t.end(), 0LL) <= from) l = mid;
                else r = mid - 1;
            }

            auto s = get(l);
            for (int u = 0; u < 50; u++) {
                t[u] -= s[u];
            }
            tot = accumulate(s.begin(), s.end(), 0LL);
            for (int u = 0; u < 50 && from > tot; u++) {
                if ((l + 1) >> u & 1) {
                    t[u]--;
                    from--;
                }
            }
            ll p = 1;
            for (int u = 0; u < 50; u++) {
                if (t[u]) {
                    p = p * power((1LL << u) % mod, t[u], mod) % mod;
                }
            }
            res[i] = p;
        }
        return res;
    }
};
```