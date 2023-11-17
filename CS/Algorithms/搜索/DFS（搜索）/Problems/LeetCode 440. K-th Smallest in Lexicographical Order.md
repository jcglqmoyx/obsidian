## [LeetCode 440. K-th Smallest in Lexicographical Order](https://leetcode.cn/problems/k-th-smallest-in-lexicographical-order/)

Given two integers `n` and `k`, return *the* `kth` *lexicographically smallest integer in the range* `[1, n]`.

**Example 1:**

**Input:** n = 13, k = 2
**Output:** 10
**Explanation:** The lexicographical order is \[1, 10, 11, 12, 13, 2, 3, 4, 5, 6, 7, 8, 9\], so the second smallest number is 10.

**Example 2:**

**Input:** n = 1, k = 1
**Output:** 1

**Constraints:**

-   `1 <= k <= n <= 1e9`
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
    int get_steps(int cur, long long n) {
        int steps = 0;
        long long first = cur, last = cur;
        while (first <= n) {
            steps += (int) (min(last, n) - first + 1);
            first *= 10, last = last * 10 + 9;
        }
        return steps;
    }

public:
    int findKthNumber(int n, int k) {
        int cur = 1;
        k--;
        while (k) {
            int step = get_steps(cur, n);
            if (step <= k) cur++, k -= step;
            else cur *= 10, k--;
        }
        return cur;
    }
};
```