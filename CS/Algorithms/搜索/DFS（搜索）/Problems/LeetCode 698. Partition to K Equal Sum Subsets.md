---
page-title: LeetCode 698. Partition to K Equal Sum Subsets
url: https://leetcode.cn/problems/partition-to-k-equal-sum-subsets/description/?envType=daily-question&envId=2024-08-25
date: 2024-08-25 20:17:20
---
Given an integer array `nums` and an integer `k`, return `true` if it is possible to divide this array into `k` non-empty subsets whose sums are all equal.

**Example 1:**

**Input:** nums = \[4,3,2,3,5,2,1\], k = 4
**Output:** true
**Explanation:** It is possible to divide it into 4 subsets (5), (1, 4), (2,3), (2,3) with equal sums.

**Example 2:**

**Input:** nums = \[1,2,3,4\], k = 3
**Output:** false

**Constraints:**

-   `1 <= k <= nums.length <= 16`
-   `1 <= nums[i] <= 104`
-   The frequency of each element is in the range `[1, 4]`.

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    bool canPartitionKSubsets(vector<int> &nums, int k) {  
        int tot = accumulate(nums.begin(), nums.end(), 0);  
        if (tot % k) {  
            return false;  
        }  
  
        int sub = tot / k;  
  
        sort(nums.begin(), nums.end(), greater<>());  
        if (nums[0] > sub) {  
            return false;  
        }  
  
        auto n = nums.size();  
        bool st[n];  
        memset(st, 0, sizeof st);  
        auto dfs = [&](auto &&dfs, int start, int cur, int need) {  
            if (!need) return true;  
            if (cur == sub) return dfs(dfs, 0, 0, need - 1);  
            for (int i = start; i < n; i++) {  
                if (!st[i] && cur + nums[i] <= sub) {  
                    st[i] = true;  
                    if (dfs(dfs, i + 1, cur + nums[i], need)) {  
                        return true;  
                    }  
                    st[i] = false;  
                    while (i + 1 < n && nums[i + 1] == nums[i]) i++;  
                    if (!cur || cur + nums[i] == sub) break;  
                }  
            }  
            return false;  
        };  
        return dfs(dfs, 0, 0, k);  
    }  
};
```

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    bool canPartitionKSubsets(vector<int> &nums, int k) {  
        int tot = accumulate(nums.begin(), nums.end(), 0);  
        if (tot % k) {  
            return false;  
        }  
  
        int sub = tot / k;  
        if (*max_element(nums.begin(), nums.end()) > sub) {  
            return false;  
        }  
  
        auto n = nums.size();  
        int s[1 << n];  
        memset(s, 0, sizeof s);  
        bool f[1 << n];  
        memset(f, 0, sizeof f);  
        f[0] = true;  
        for (int i = 0; i < 1 << n; i++) {  
            if (!f[i]) continue;  
            for (int j = 0; j < n; j++) {  
                if (!(i >> j & 1) && s[i] + nums[j] <= sub) {  
                    int u = i | (1 << j);  
                    s[u] = s[i] + nums[j];  
                    if (s[u] == sub) s[u] = 0;  
                    f[u] = true;  
                }  
            }  
        }  
        return f[(1 << n) - 1];  
    }  
};
```