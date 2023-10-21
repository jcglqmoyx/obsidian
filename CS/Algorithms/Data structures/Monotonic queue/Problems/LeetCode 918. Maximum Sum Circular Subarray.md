## [LeetCode 918. Maximum Sum Circular Subarray](https://leetcode.cn/problems/maximum-sum-circular-subarray/)

Given a **circular integer array** `nums` of length `n`, return *the maximum possible sum of a non-empty **subarray** of* `nums`.

A **circular array** means the end of the array connects to the beginning of the array. Formally, the next element of `nums[i]` is `nums[(i + 1) % n]` and the previous element of `nums[i]` is `nums[(i - 1 + n) % n]`.

A **subarray** may only include each element of the fixed buffer `nums` at most once. Formally, for a subarray `nums[i], nums[i + 1], ..., nums[j]`, there does not exist `i <= $k_1$`, $k_2$ `<= j` with $`k_1$ % n == $k_2$ % n`.

**Example 1:**

```
**Input**: nums = [1,-2,3,-2]
**Output**: 3
**Explanation**: Subarray [3] has maximum sum 3.
```

**Example 2:**

```
**Input**: nums = [5,-3,5]
**Output**: 10
**Explanation**: Subarray [5,5] has maximum sum 5 + 5 = 10.
```

**Example 3:**

```
**Input**: nums = [-3,-2,-3]
**Output**: -2
**Explanation**: Subarray [-2] has maximum sum -2.
```

**Constraints:**

- `n == nums.length`
- `1 <= n <= 3 * $10^4$`
- `3 * $10^4$ <= nums[i] <= 3 * $10^4$`

### C++

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int maxSubarraySumCircular(vector<int> &nums) {
        int n = (int) nums.size(), res = INT32_MIN;
        int s[n * 2];
        memset(s, 0, sizeof s);
        for (int i = 1; i < n * 2; i++) {
            s[i] = s[i - 1] + nums[(i - 1) % n];
        }
        deque<int> dq = {0};
        for (int i = 1; i < n * 2; i++) {
            while (!dq.empty() && i - dq.front() > n) dq.pop_front();
            if (!dq.empty()) res = max(res, s[i] - s[dq.front()]);
            while (!dq.empty() && s[dq.back()] >= s[i]) dq.pop_back();
            dq.push_back(i);
        }
        return res;
    }
};
```

### Python3

```cpp
from typing import List

class Solution:
    def maxSubarraySumCircular(self, nums: List[int]) -> int:
        n, res = len(nums), float('-inf')
        s = [0] * (n * 2)
        for i in range(1, n * 2):
            s[i] = s[i - 1] + nums[(i - 1) % n]
        q = [0]
        for i in range(1, n * 2):
            while q and i - q[0] > n:
                q = q[1:]
            if q:
                res = max(res, s[i] - s[q[0]])
            while q and s[q[-1]] >= s[i]:
                q = q[:-1]
            q.append(i)
        return res
```