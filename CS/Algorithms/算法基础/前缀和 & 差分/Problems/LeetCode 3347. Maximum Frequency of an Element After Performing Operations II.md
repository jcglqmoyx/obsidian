---
page-title: LeetCode 3347. Maximum Frequency of an Element After Performing Operations II
url: https://leetcode.cn/problems/maximum-frequency-of-an-element-after-performing-operations-ii/description/
date: 2024-11-14 18:09:52
---
You are given an integer array `nums` and two integers `k` and `numOperations`.  
  
You must perform an **operation** `numOperations` times on `nums`, where in each operation you:  
  
- Select an index `i` that was **not** selected in any previous operations.  
- Add an integer in the range `[-k, k]` to `nums[i]`.  
  
Return the **maximum** possible frequency of any element in `nums` after performing the **operations**.  
  
**Example 1:**  
  
**Input:** nums = \[1,4,5\], k = 1, numOperations = 2  
  
**Output:** 2  
  
**Explanation:**  
  
We can achieve a maximum frequency of two by:  
  
- Adding 0 to `nums[1]`, after which `nums` becomes `[1, 4, 5]`.  
- Adding -1 to `nums[2]`, after which `nums` becomes `[1, 4, 4]`.  
  
**Example 2:**  
  
**Input:** nums = \[5,11,20,20\], k = 5, numOperations = 1  
  
**Output:** 2  
  
**Explanation:**  
  
We can achieve a maximum frequency of two by:  
  
- Adding 0 to `nums[1]`.  
  
**Constraints:**  
  
- `1 <= nums.length <= 105`  
- `1 <= nums[i] <= 109`  
- `0 <= k <= 109`  
- `0 <= numOperations <= nums.length`

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int maxFrequency(vector<int> &nums, int k, int numOperations) {  
        map<int, int> diff;  
        unordered_map<int, int> cnt;  
        for (int x: nums) {  
            cnt[x]++;  
            diff[x];  
            diff[x - k]++;  
            diff[x + k + 1]--;  
        }  
        int res = 0, sum = 0;  
        for (auto &[x, v]: diff) {  
            sum += v;  
            res = max(res, min(sum, cnt[x] + numOperations));  
        }  
        return res;  
    }  
};
```