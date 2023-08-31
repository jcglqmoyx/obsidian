---
page-title: "673. 最长递增子序列的个数 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/number-of-longest-increasing-subsequence/
date: "2023-07-21 10:33:41"
---
Given an integer array `nums`, return *the number of longest increasing subsequences.*

**Notice** that the sequence has to be **strictly** increasing.

**Example 1:**

**Input:** nums = \[1,3,5,4,7\]
**Output:** 2
**Explanation:** The two longest increasing subsequences are \[1, 3, 4, 7\] and \[1, 3, 5, 7\].

**Example 2:**

**Input:** nums = \[2,2,2,2,2\]
**Output:** 5
**Explanation:** The length of the longest increasing subsequence is 1, and there are 5 increasing subsequences of length 1, so output 5.

**Constraints:**

-   `1 <= nums.length <= 2000`
-   `-106 <= nums[i] <= 106`

O(n^2):
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int findNumberOfLIS(vector<int> &nums) {
        int n = (int) nums.size(), f[n], g[n], max_len = 0, res = 0;
        for (int i = 0; i < n; i++) {
            f[i] = g[i] = 1;
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    if (f[i] < f[j] + 1) {
                        f[i] = f[j] + 1;
                        g[i] = g[j];
                    } else if (f[i] == f[j] + 1) {
                        g[i] += g[j];
                    }
                }
            }
            if (max_len < f[i]) {
                max_len = f[i];
                res = g[i];
            } else if (max_len == f[i]) {
                res += g[i];
            }
        }
        return res;
    }
};
```


O(n * log(n))
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
    int binarySearch(int n, function<bool(int)> f) {
        int l = 0, r = n;
        while (l < r) {
            int mid = (l + r) / 2;
            if (f(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }

public:
    int findNumberOfLIS(vector<int> &nums) {
        vector<vector<int>> d, cnt;
        for (int v: nums) {
            int i = binarySearch(d.size(), [&](int i) { return d[i].back() >= v; });
            int c = 1;
            if (i > 0) {
                int k = binarySearch(d[i - 1].size(), [&](int k) { return d[i - 1][k] < v; });
                c = cnt[i - 1].back() - cnt[i - 1][k];
            }
            if (i == d.size()) {
                d.push_back({v});
                cnt.push_back({0, c});
            } else {
                d[i].push_back(v);
                cnt[i].push_back(cnt[i].back() + c);
            }
        }
        return cnt.back().back();
    }
};
```