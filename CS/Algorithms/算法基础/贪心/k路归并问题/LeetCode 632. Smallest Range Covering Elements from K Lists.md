---
page-title: "Smallest Range Covering Elements from K Lists - LeetCode"
url: https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/description/
date: "2023-11-27 09:29:02"
---
You have `k` lists of sorted integers in **non-decreasing order**. Find the **smallest** range that includes at least one number from each of the `k` lists.

We define the range `[a, b]` is smaller than range `[c, d]` if `b - a < d - c` **or** `a < c` if `b - a == d - c`.

**Example 1:**

**Input:** nums = \[\[4,10,15,24,26\],\[0,9,12,20\],\[5,18,22,30\]\]
**Output:** \[20,24\]
**Explanation:** 
List 1: \[4, 10, 15, 24,26\], 24 is in range \[20,24\].
List 2: \[0, 9, 12, 20\], 20 is in range \[20,24\].
List 3: \[5, 18, 22, 30\], 22 is in range \[20,24\].

**Example 2:**

**Input:** nums = \[\[1,2,3\],\[1,2,3\],\[1,2,3\]\]
**Output:** \[1,1\]

**Constraints:**

-   `nums.length == k`
-   `1 <= k <= 3500`
-   `1 <= nums[i].length <= 50`
-   `-105 <= nums[i][j] <= 105`
-   `nums[i]` is sorted in **non-decreasing** order.

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    vector<int> smallestRange(vector<vector<int>>&nums) {
        struct T {
            int val;
            int row, col;

            bool operator<(const T&t) const {
                return val > t.val;
            }
        };
        priority_queue<T> q;
        int mx = INT_MIN;
        for (int i = 0; i < nums.size(); i++) {
            q.push({nums[i][0], i, 0});
            mx = max(mx, nums[i][0]);
        }
        vector<int> res;
        while (!q.empty()) {
            auto t = q.top();
            q.pop();
            int l = t.val, r = mx;
            if (res.empty() || r - l < res[1] - res[0]) {
                res = {l, r};
            }
            if (t.col + 1 < nums[t.row].size()) {
                int next = nums[t.row][t.col + 1];
                q.push({next, t.row, t.col + 1});
                mx = max(mx, next);
            } else {
                break;
            }
        }
        return res;
    }
};
```