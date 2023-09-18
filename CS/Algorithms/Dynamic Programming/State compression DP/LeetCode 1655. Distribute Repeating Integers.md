## [LeetCode 1655. Distribute Repeating Integers](https://leetcode.cn/problems/distribute-repeating-integers/description/)
You are given an array of `n` integers, `nums`, where there are at most `50` unique values in the array. You are also given an array of `m` customer order quantities, `quantity`, where `quantity[i]` is the amount of integers the `ith` customer ordered. Determine if it is possible to distribute `nums` such that:

-   The `ith` customer gets **exactly** `quantity[i]` integers,
-   The integers the `ith` customer gets are **all equal**, and
-   Every customer is satisfied.

Return `true` *if it is possible to distribute* `nums` *according to the above conditions*.

**Example 1:**

**Input:** nums = \[1,2,3,4\], quantity = \[2\]
**Output:** false
**Explanation:** The 0th customer cannot be given two different integers.

**Example 2:**

**Input:** nums = \[1,2,3,3\], quantity = \[2\]
**Output:** true
**Explanation:** The 0th customer is given \[3,3\]. The integers \[1,2\] are not used.

**Example 3:**

**Input:** nums = \[1,1,2,2\], quantity = \[2,2\]
**Output:** true
**Explanation:** The 0th customer is given \[1,1\], and the 1st customer is given \[2,2\].

**Constraints:**

-   `n == nums.length`
-   `1 <= n <= 105`
-   `1 <= nums[i] <= 1000`
-   `m == quantity.length`
-   `1 <= m <= 10`
-   `1 <= quantity[i] <= 105`
-   There are at most `50` unique values in `nums`.
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    bool canDistribute(vector<int> &nums, vector<int> &quantity) {  
        unordered_map<int, int> cnt;  
        for (int x: nums) cnt[x]++;  
        int a[55] = {}, idx = 1;  
        for (auto[k, v]: cnt) {  
            a[idx++] = v;  
        }  
        int n = idx, m = (int) quantity.size();  
        int s[1 << m];  
        memset(s, 0, sizeof s);  
        for (int i = 0; i < 1 << m; i++) {  
            for (int j = 0; j < m; j++) {  
                if (i >> j & 1) {  
                    s[i] += quantity[j];  
                }  
            }  
        }  
        int f[n + 1][1 << m];  
        memset(f, 0, sizeof f);  
        f[0][0] = 1;  
        for (int i = 0; i < n; i++) {  
            for (int j = 0; j < 1 << m; j++) {  
                if (f[i][j]) {  
                    f[i + 1][j] = true;  
                    for (int t = j ^ (1 << m) - 1, k = t; k; k = (k - 1) & t) {  
                        if (s[k] <= a[i + 1]) {  
                            f[i + 1][j | k] = true;  
                        }  
                    }  
                }  
            }  
        }  
        return f[n][(1 << m) - 1];  
    }  
};
```