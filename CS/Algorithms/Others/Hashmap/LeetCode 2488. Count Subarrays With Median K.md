---
page-title: "2488. 统计中位数为 K 的子数组 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/count-subarrays-with-median-k/
date: "2023-10-15 22:19:44"
---
You are given an array `nums` of size `n` consisting of **distinct** integers from `1` to `n` and a positive integer `k`.

Return *the number of non-empty subarrays in* `nums` *that have a **median** equal to* `k`.

**Note**:

-   The median of an array is the **middle** element after sorting the array in **ascending** order. If the array is of even length, the median is the **left** middle element.
    -   For example, the median of `[2,3,1,4]` is `2`, and the median of `[8,4,3,5,1]` is `4`.
-   A subarray is a contiguous part of an array.

**Example 1:**

**Input:** nums = \[3,2,1,4,5\], k = 4
**Output:** 3
**Explanation:** The subarrays that have a median equal to 4 are: \[4\], \[4,5\] and \[1,4,5\].

**Example 2:**

**Input:** nums = \[2,3,1\], k = 3
**Output:** 1
**Explanation:** \[3\] is the only subarray that has a median equal to 3.

**Constraints:**

-   `n == nums.length`
-   `1 <= n <= 105`
-   `1 <= nums[i], k <= n`
-   The integers in `nums` are distinct.

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int countSubarrays(vector<int> &nums, int k) {
        int n = (int) nums.size(), res = 1;
        int idx = 0;
        for (int i = 1; i < n; i++) {
            if (nums[i] == k) {
                idx = i;
                break;
            }
        }
        unordered_map<int, int> cnt;
        int lt = 0, gt = 0;
        for (int i = idx - 1; i >= 0; i--) {
            if (nums[i] < k) lt++;
            else gt++;
            if (lt == gt || lt + 1 == gt) res++;
            cnt[lt - gt]++;
        }
        lt = 0, gt = 0;
        for (int i = idx + 1; i < n; i++) {
            if (nums[i] < k) lt++;
            else gt++;
            res += cnt[gt - lt] + cnt[gt - lt - 1];
            if (lt == gt || lt + 1 == gt) res++;
        }
        return res;
    }
};
```