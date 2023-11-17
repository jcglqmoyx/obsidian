---
page-title: "Subarrays Distinct Element Sum of Squares II - LeetCode"
url: https://leetcode.com/problems/subarrays-distinct-element-sum-of-squares-ii/description/
date: "2023-11-16 09:39:15"
---
You are given a **0-indexed** integer array `nums`.

The **distinct count** of a subarray of `nums` is defined as:

-   Let `nums[i..j]` be a subarray of `nums` consisting of all the indices from `i` to `j` such that `0 <= i <= j < nums.length`. Then the number of distinct values in `nums[i..j]` is called the distinct count of `nums[i..j]`.

Return *the sum of the **squares** of **distinct counts** of all subarrays of* `nums`.

Since the answer may be very large, return it **modulo** `109 + 7`.

A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = \[1,2,1\]
**Output:** 15
**Explanation:** Six possible subarrays are:
\[1\]: 1 distinct value
\[2\]: 1 distinct value
\[1\]: 1 distinct value
\[1,2\]: 2 distinct values
\[2,1\]: 2 distinct values
\[1,2,1\]: 2 distinct values
The sum of the squares of the distinct counts in all subarrays is equal to 12 + 12 + 12 + 22 + 22 + 22 = 15.

**Example 2:**

**Input:** nums = \[2,2\]
**Output:** 3
**Explanation:** Three possible subarrays are:
\[2\]: 1 distinct value
\[2\]: 1 distinct value
\[2,2\]: 1 distinct value
The sum of the squares of the distinct counts in all subarrays is equal to 12 + 12 + 12 = 3.

**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 105`

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 100010, MOD = 1e9 + 7;

struct Node {
    int l, r;
    long long s1, s2;
    int add;
} tr[N << 2];

void push_up(int u) {
    auto &root = tr[u], &l = tr[u << 1], &r = tr[u << 1 | 1];
    root.s1 = l.s1 + r.s1;
    root.s2 = (l.s2 + r.s2) % MOD;
}

void push_down(int u) {
    Node &root = tr[u], &l = tr[u << 1], &r = tr[u << 1 | 1];
    if (root.add) {
        l.add += root.add;
        r.add += root.add;
        l.s2 = (l.s2 + l.s1 * 2 * root.add + root.add * root.add * (l.r - l.l + 1)) % MOD;
        r.s2 = (r.s2 + r.s1 * 2 * root.add + root.add * root.add * (r.r - r.l + 1)) % MOD;
        l.s1 += (l.r - l.l + 1) * root.add;
        r.s1 += (r.r - r.l + 1) * root.add;
        root.add = 0;
    }
}

void build(int u, int l, int r) {
    tr[u] = {l, r};
    if (l != r) {
        int mid = (l + r) >> 1;
        build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
    }
}

void update(int u, int l, int r) {
    if (tr[u].l >= l && tr[u].r <= r) {
        tr[u].s2 = (tr[u].s2 + tr[u].s1 * 2 + tr[u].r - tr[u].l + 1) % MOD;
        tr[u].s1 += tr[u].r - tr[u].l + 1;
        tr[u].add += 1;
    } else {
        push_down(u);
        int mid = (tr[u].l + tr[u].r) >> 1;
        if (l <= mid) update(u << 1, l, r);
        if (r > mid) update(u << 1 | 1, l, r);
        push_up(u);
    }
}

class Solution {
public:
    int sumCounts(vector<int> &nums) {
        int n = (int) nums.size();
        build(1, 0, n - 1);
        int last[N];
        memset(last, -1, sizeof last);
        int res = 0;
        for (int i = 0; i < n; i++) {
            update(1, last[nums[i]] + 1, i);
            res = (res + tr[1].s2) % MOD;
            last[nums[i]] = i;
        }
        return res;
    }
};
```