## [LeetCode **491. Non-decreasing Subsequences](https://leetcode.cn/problems/non-decreasing-subsequences/description/) 

Given an integer array `nums`, return *all the different possible non-decreasing subsequences of the given array with at least two elements*. You may return the answer in **any order**.

**Example 1:**

```
**Input**: nums = [4,6,7,7]
**Output**: [[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]
```

**Example 2:**

```
**Input**: nums = [4,4,3,2,1]
**Output**: [[4,4]]
```

**Constraints:**

- `1 <= nums.length <= 15`
- `100 <= nums[i] <= 100`

### C++

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
    vector<int> path;
    vector<vector<int>> paths;
    vector<int> arr;

    void dfs(int cur, int prev) {
        if (cur == arr.size()) {
            if (path.size() >= 2) {
                paths.push_back(path);
            }
            return;
        }
        if (arr[cur] >= prev) {
            path.push_back(arr[cur]);
            dfs(cur + 1, arr[cur]);
            path.pop_back();
        }
        if (arr[cur] != prev) {
            dfs(cur + 1, prev);
        }
    }

public:
    vector<vector<int>> findSubsequences(vector<int> &nums) {
        arr = nums;
        dfs(0, INT32_MIN);
        return paths;
    }
};
```

### Python3

```cpp
from typing import List

class Solution:
    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        path, res = [], []

        def dfs(u: int) -> None:
            if u == len(nums):
                if len(path) >= 2:
                    res.append(path[:])
                return
            if not path or path[-1] < nums[u]:
                dfs(u + 1)

                path.append(nums[u])
                dfs(u + 1)
                path.pop()
            elif path[-1] == nums[u]:
                path.append(nums[u])
                dfs(u + 1)
                path.pop()
            else:
                dfs(u + 1)

        dfs(0)
        return res
```