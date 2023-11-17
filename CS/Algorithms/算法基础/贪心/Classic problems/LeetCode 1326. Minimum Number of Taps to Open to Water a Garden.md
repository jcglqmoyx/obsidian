## [LeetCode 1326. Minimum Number of Taps to Open to Water a Garden](https://leetcode.cn/problems/minimum-number-of-taps-to-open-to-water-a-garden/)

There is a one-dimensional garden on the x-axis. The garden starts at the point `0` and ends at the point `n`. (i.e The length of the garden is `n`).

There are `n + 1` taps located at points `[0, 1, ..., n]` in the garden.

Given an integer `n` and an integer array `ranges` of length `n + 1` where `ranges[i]` (0-indexed) means the `i-th` tap can water the area `[i - ranges[i], i + ranges[i]]` if it was open.

Return _the minimum number of taps_ that should be open to water the whole garden, If the garden cannot be watered return **-1**.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/01/16/1685_example_1.png](https://assets.leetcode.com/uploads/2020/01/16/1685_example_1.png)

```
Input: n = 5, ranges = [3,4,1,1,0,0]
Output: 1
Explanation: The tap at point 0 can cover the interval [-3,3]
The tap at point 1 can cover the interval [-3,5]
The tap at point 2 can cover the interval [1,3]
The tap at point 3 can cover the interval [2,4]
The tap at point 4 can cover the interval [4,4]
The tap at point 5 can cover the interval [5,5]
Opening Only the second tap will water the whole garden [0,5]
```

**Example 2:**

```
Input: n = 3, ranges = [0,0,0,0]
Output: -1
Explanation: Even if you activate all the four taps you cannot water the whole garden.
```

**Constraints:**

-   `1 <= n <= $10^4$`
-   `ranges.length == n + 1`
-   `0 <= ranges[i] <= 100`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int minTaps(int n, vector<int> &ranges) {
        int right_most[n + 1];
        iota(right_most, right_most + n + 1, 0);
        for (int i = 0; i < n + 1; i++) {
            int l = max(0, i - ranges[i]), r = min(n, i + ranges[i]);
//            right_most[l] = max(right_most[l], r);
            right_most[l] = r;
        }
        int res = 0, last = 0, pre = 0;
        for (int i = 0; i < n; i++) {
            last = max(last, right_most[i]);
            if (i == last) return -1;
            if (i == pre) res++, pre = last;
        }
        return res;
    }
};
```