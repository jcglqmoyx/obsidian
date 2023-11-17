## [2638. Count the Number of K-Free Subsets](https://leetcode.cn/problems/count-the-number-of-k-free-subsets/)

You are given an integer array `nums`, which contains **distinct** elements and an integer `k`.

A subset is called a **k-Free** subset if it contains **no** two elements with an absolute difference equal to `k`. Notice that the empty set is a **k-Free** subset.

Return *the number of **k-Free** subsets of* `nums`.

A **subset** of an array is a selection of elements (possibly none) of the array.

**Example 1:**

**Input:** nums = \[5,4,6\], k = 1
**Output:** 5
**Explanation:** There are 5 valid subsets: {}, {5}, {4}, {6} and {4, 6}.

**Example 2:**

**Input:** nums = \[2,3,5,8\], k = 5
**Output:** 12
**Explanation:** There are 12 valid subsets: {}, {2}, {3}, {5}, {8}, {2, 3}, {2, 3, 5}, {2, 5}, {2, 5, 8}, {2, 8}, {3, 5} and {5, 8}.

**Example 3:**

**Input:** nums = \[10,5,9,11\], k = 20
**Output:** 16
**Explanation:** All subsets are valid. Since the total count of subsets is 24 \= 16, so the answer is 16. 

**Constraints:**

-   `1 <= nums.length <= 50`
-   `1 <= nums[i] <= 1000`
-   `1 <= k <= 1000`
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    long long countTheNumOfKFreeSubsets(vector<int> &nums, int k) {  
        sort(nums.begin(), nums.end());  
        unordered_map<int, vector<int>> map;  
        for (int x: nums) map[x % k].push_back(x);  
        long long res = 1;  
        for (auto &[_, v]: map) {  
            long long f0 = 1, f1 = 1;  
            int last = v[0];  
            for (int i = 1; i < v.size(); i++) {  
                int x = v[i];  
                long long g0 = f0 + f1, g1 = f0 + f1;  
                if (x - last == k) g1 = f0;  
                f0 = g0, f1 = g1;  
                last = x;  
            }  
            res *= f0 + f1;  
        }  
        return res;  
    }  
};
```