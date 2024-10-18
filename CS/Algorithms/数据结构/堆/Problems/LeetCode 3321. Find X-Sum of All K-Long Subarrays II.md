---
page-title: LeetCode 3321. Find X-Sum of All K-Long Subarrays II
url: https://leetcode.com/problems/find-x-sum-of-all-k-long-subarrays-ii/description/
date: 2024-10-16 22:45:55
---
You are given an array `nums` of `n` integers and two integers `k` and `x`.

The **x-sum** of an array is calculated by the following procedure:

-   Count the occurrences of all elements in the array.
-   Keep only the occurrences of the top `x` most frequent elements. If two elements have the same number of occurrences, the element with the **bigger** value is considered more frequent.
-   Calculate the sum of the resulting array.

**Note** that if an array has less than `x` distinct elements, its **x-sum** is the sum of the array.

Return an integer array `answer` of length `n - k + 1` where `answer[i]` is the **x-sum** of the subarray `nums[i..i + k - 1]`.

**Example 1:**

**Input:** nums = \[1,1,2,2,3,4,2,3\], k = 6, x = 2

**Output:** \[6,10,12\]

**Explanation:**

-   For subarray `[1, 1, 2, 2, 3, 4]`, only elements 1 and 2 will be kept in the resulting array. Hence, `answer[0] = 1 + 1 + 2 + 2`.
-   For subarray `[1, 2, 2, 3, 4, 2]`, only elements 2 and 4 will be kept in the resulting array. Hence, `answer[1] = 2 + 2 + 2 + 4`. Note that 4 is kept in the array since it is bigger than 3 and 1 which occur the same number of times.
-   For subarray `[2, 2, 3, 4, 2, 3]`, only elements 2 and 3 are kept in the resulting array. Hence, `answer[2] = 2 + 2 + 2 + 3 + 3`.

**Example 2:**

**Input:** nums = \[3,8,7,8,7,5\], k = 2, x = 2

**Output:** \[11,15,15,15,12\]

**Explanation:**

Since `k == x`, `answer[i]` is equal to the sum of the subarray `nums[i..i + k - 1]`.

**Constraints:**

-   `nums.length == n`
-   `1 <= n <= 105`
-   `1 <= nums[i] <= 109`
-   `1 <= x <= k <= nums.length`

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    vector<long long> findXSum(vector<int> &nums, int k, int x) {  
        unordered_map<int, int> cnt;  
        set<pair<int, int>> s1, s2;  
        long long sum = 0;  
        auto l2r = [&]() {  
            if (s1.empty()) return;  
            sum += s1.rbegin()->first * 1LL * s1.rbegin()->second;  
            s2.emplace(*s1.rbegin());  
            s1.erase(s1.find(*s1.rbegin()));  
        };  
        auto r2l = [&]() {  
            if (s2.empty()) return;  
            sum -= s2.begin()->first * 1LL * s2.begin()->second;  
            s1.emplace(*s2.begin());  
            s2.erase(s2.begin());  
        };  
        vector<long long> res;  
        for (int l = 0, r = 0; r < nums.size(); r++) {  
            cnt[nums[r]]++;  
            pair<int, int> old_pair = {cnt[nums[r]] - 1, nums[r]};  
            pair<int, int> new_pair = {cnt[nums[r]], nums[r]};  
            if (s1.contains(old_pair)) {  
                s1.erase(old_pair);  
                s1.emplace(new_pair);  
            } else {  
                s2.erase(old_pair);  
                s2.emplace(new_pair);  
                sum += nums[r];  
            }  
            l2r();  
            r2l();  
            while (s2.size() > x) {  
                r2l();  
            }  
            while (!s1.empty() && s2.size() < x) {  
                l2r();  
                r2l();  
                l2r();  
            }  
            if (r >= k - 1) {  
                res.emplace_back(sum);  
                cnt[nums[l]]--;  
                old_pair = {cnt[nums[l]] + 1, nums[l]};  
                new_pair = {cnt[nums[l]], nums[l]};  
                if (s1.contains(old_pair)) {  
                    s1.erase(old_pair);  
                    s1.emplace(new_pair);  
                } else {  
                    s2.erase(old_pair);  
                    s2.emplace(new_pair);  
                    sum -= nums[l];  
                }  
                l++;  
            }  
        }  
        return res;  
    }  
};
```