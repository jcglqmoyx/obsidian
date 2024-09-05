---
page-title: LeetCode 2818. Apply Operations to Maximize Score
url: https://leetcode.com/problems/apply-operations-to-maximize-score/description/
date: 2024-09-05 23:07:09
---
You are given an array `nums` of `n` positive integers and an integer `k`.

Initially, you start with a score of `1`. You have to maximize your score by applying the following operation at most `k` times:

-   Choose any **non-empty** subarray `nums[l, ..., r]` that you haven't chosen previously.
-   Choose an element `x` of `nums[l, ..., r]` with the highest **prime score**. If multiple such elements exist, choose the one with the smallest index.
-   Multiply your score by `x`.

Here, `nums[l, ..., r]` denotes the subarray of `nums` starting at index `l` and ending at the index `r`, both ends being inclusive.

The **prime score** of an integer `x` is equal to the number of distinct prime factors of `x`. For example, the prime score of `300` is `3` since `300 = 2 * 2 * 3 * 5 * 5`.

Return *the **maximum possible score** after applying at most* `k` *operations*.

Since the answer may be large, return it modulo `109 + 7`.

**Example 1:**

**Input:** nums = \[8,3,9,3,8\], k = 2
**Output:** 81
**Explanation:** To get a score of 81, we can apply the following operations:
- Choose subarray nums\[2, ..., 2\]. nums\[2\] is the only element in this subarray. Hence, we multiply the score by nums\[2\]. The score becomes 1 \* 9 = 9.
- Choose subarray nums\[2, ..., 3\]. Both nums\[2\] and nums\[3\] have a prime score of 1, but nums\[2\] has the smaller index. Hence, we multiply the score by nums\[2\]. The score becomes 9 \* 9 = 81.
It can be proven that 81 is the highest score one can obtain.

**Example 2:**

**Input:** nums = \[19,12,14,6,10,18\], k = 3
**Output:** 4788
**Explanation:** To get a score of 4788, we can apply the following operations: 
- Choose subarray nums\[0, ..., 0\]. nums\[0\] is the only element in this subarray. Hence, we multiply the score by nums\[0\]. The score becomes 1 \* 19 = 19.
- Choose subarray nums\[5, ..., 5\]. nums\[5\] is the only element in this subarray. Hence, we multiply the score by nums\[5\]. The score becomes 19 \* 18 = 342.
- Choose subarray nums\[2, ..., 3\]. Both nums\[2\] and nums\[3\] have a prime score of 2, but nums\[2\] has the smaller index. Hence, we multipy the score by nums\[2\]. The score becomes 342 \* 14 = 4788.
It can be proven that 4788 is the highest score one can obtain.

**Constraints:**

-   `1 <= nums.length == n <= 105`
-   `1 <= nums[i] <= 105`
-   `1 <= k <= min(n * (n + 1) / 2, 109)`


```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
    const int MOD = 1e9 + 7;

    long long power(long long x, int n) {
        long long res = 1;
        while (n) {
            if (n & 1) res = res * x % MOD;
            x = x * x % MOD;
            n >>= 1;
        }
        return res;
    }

public:
    int maximumScore(vector<int> &nums, int k) {
        int n = nums.size();
        int score[n];
        memset(score, 0, sizeof score);
        for (int i = 0; i < n; i++) {
            int x = nums[i];
            for (int a = 2; a * a <= x; a++) {
                if (x % a == 0) {
                    score[i]++;
                    while (x % a == 0) {
                        x /= a;
                    }
                }
            }
            if (x > 1) {
                score[i]++;
            }
        }
        int stk[n];
        int l[n], r[n];
        for (int tt = -1, i = 0; i < n; i++) {
            while (~tt && score[stk[tt]] < score[i]) tt--;
            l[i] = ~tt ? stk[tt] + 1 : 0;
            stk[++tt] = i;
        }
        for (int tt = -1, i = n - 1; ~i; i--) {
            while (~tt && score[stk[tt]] <= score[i]) tt--;
            r[i] = ~tt ? stk[tt] - 1 : n - 1;
            stk[++tt] = i;
        }
        int id[n];
        iota(id, id + n, 0);
        sort(id, id + n, [&](int i, int j) {
            return nums[i] > nums[j];
        });
        long long res = 1;
        for (int i = 0; i < n && k; i++) {
            long long left = id[i] - l[id[i]] + 1, right = r[id[i]] - id[i] + 1;
            long long t = min((long long) k, left * right);
            res = res * power(nums[id[i]], t) % MOD;
            k -= t;
        }
        return res;
    }
};
```