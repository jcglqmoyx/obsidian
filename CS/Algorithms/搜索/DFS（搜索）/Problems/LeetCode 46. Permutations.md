---
page-title: "46. 全排列 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/permutations/
date: "2023-10-20 09:37:39"
---
Given an array `nums` of distinct integers, return *all the possible permutations*. You can return the answer in **any order**.

**Example 1:**

**Input:** nums = \[1,2,3\]
**Output:** \[\[1,2,3\],\[1,3,2\],\[2,1,3\],\[2,3,1\],\[3,1,2\],\[3,2,1\]\]

**Example 2:**

**Input:** nums = \[0,1\]
**Output:** \[\[0,1\],\[1,0\]\]

**Example 3:**

**Input:** nums = \[1\]
**Output:** \[\[1\]\]

**Constraints:**

-   `1 <= nums.length <= 6`
-   `-10 <= nums[i] <= 10`
-   All the integers of `nums` are **unique**.

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
    int n;

    void dfs(vector<int> &nums, vector<vector<int>> &res, vector<int> &path, vector<bool> &st, int u) {
        if (u == nums.size()) {
            res.push_back(path);
        } else {
            for (int i = 0; i < n; i++) {
                if (!st[i]) {
                    st[i] = true;
                    path.push_back(nums[i]);
                    dfs(nums, res, path, st, u + 1);
                    path.pop_back();
                    st[i] = false;
                }
            }
        }
    }

public:
    vector<vector<int>> permute(vector<int> &nums) {
        n = (int) nums.size();
        vector<vector<int>> res;
        vector<int> path;
        vector<bool> st(n);
        dfs(nums, res, path, st, 0);
        return res;
    }
};
```