---
page-title: 3165. Maximum Sum of Subsequence With Non-adjacent Elements
url: https://leetcode.com/problems/maximum-sum-of-subsequence-with-non-adjacent-elements/description/
date: 2024-05-27 10:49:16
---
You are given an array `nums` consisting of integers. You are also given a 2D array `queries`, where `queries[i] = [posi, xi]`.

For query `i`, we first set `nums[posi]` equal to `xi`, then we calculate the answer to query `i` which is the **maximum** sum of a subsequence

of `nums` where **no two adjacent elements are selected**.

Return the *sum* of the answers to all queries.

Since the final answer may be very large, return it **modulo** `109 + 7`.

A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** nums = \[3,5,9\], queries = \[\[1,-2\],\[0,-3\]\]

**Output:** 21

**Explanation:**  
After the 1st query, `nums = [3,-2,9]` and the maximum sum of a subsequence with non-adjacent elements is `3 + 9 = 12`.  
After the 2nd query, `nums = [-3,-2,9]` and the maximum sum of a subsequence with non-adjacent elements is 9.

**Example 2:**

**Input:** nums = \[0,-1\], queries = \[\[0,-5\]\]

**Output:** 0

**Explanation:**  
After the 1st query, `nums = [-5,-1]` and the maximum sum of a subsequence with non-adjacent elements is 0 (choosing an empty subsequence).

**Constraints:**

-   `1 <= nums.length <= 5 * 104`
-   `-105 <= nums[i] <= 105`
-   `1 <= queries.length <= 5 * 104`
-   `queries[i] == [posi, xi]`
-   `0 <= posi <= nums.length - 1`
-   `-105 <= xi <= 105`

```cpp
#include <bits/stdc++.h>

using namespace std;

const int MOD = 1e9 + 7;

const int N = 50010;

struct Node {
    int l, r;
    unsigned int f00, f01, f10, f11;
} tr[N << 2];

void push_up(int u) {
    auto &root = tr[u], &l = tr[u << 1], &r = tr[u << 1 | 1];
    root.f00 = max(l.f00 + r.f10, l.f01 + r.f00);
    root.f01 = max(l.f00 + r.f11, l.f01 + r.f01);
    root.f10 = max(l.f10 + r.f10, l.f11 + r.f00);
    root.f11 = max(l.f11 + r.f01, l.f10 + r.f11);
}

void build(vector<int> &nums, int u, int l, int r) {
    tr[u] = {l, r};
    if (l == r) {
        tr[u].f11 = max(nums[l], 0);
    } else {
        int mid = (l + r) >> 1;
        build(nums, u << 1, l, mid);
        build(nums, u << 1 | 1, mid + 1, r);
        push_up(u);
    }
}

void update(int u, int p, int v) {
    if (tr[u].l == tr[u].r) tr[u].f11 = max(v, 0);
    else {
        int mid = (tr[u].l + tr[u].r) >> 1;
        if (p <= mid) update(u << 1, p, v);
        else update(u << 1 | 1, p, v);
        push_up(u);
    }
}

class Solution {
public:
    int maximumSumSubsequence(vector<int> &nums, vector<vector<int>> &queries) {
        int n = nums.size();
        build(nums, 1, 0, n - 1);
        unsigned int res = 0;
        for (auto &q: queries) {
            update(1, q[0], q[1]);
            res = (res + tr[1].f11) % MOD;
        }
        return res;
    }
};
```