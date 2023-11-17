## [LeetCode 47. Permutations II](https://leetcode.cn/problems/permutations-ii/)

Given a collection of numbers, `nums`, that might contain duplicates, return *all possible unique permutations **in any order**.*

**Example 1:**

```
**Input**: nums = [1,1,2]
**Output**:
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

**Example 2:**

```
**Input**: nums = [1,2,3]
**Output**: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**Constraints:**

- `1 <= nums.length <= 8`
- `10 <= nums[i] <= 10`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
    int n;
    vector<bool> st;
    vector<int> path;
    vector<vector<int>> res;

    void dfs(vector<int> &nums, int u) {
        if (u == n) {
            res.push_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); i++) {
            if (st[i] || i > 0 && nums[i] == nums[i - 1] && !st[i - 1]) continue;
            path[u] = nums[i];
            st[i] = true;
            dfs(nums, u + 1);
            st[i] = false;
        }
    }

public:
    vector<vector<int>> permuteUnique(vector<int> &nums) {
        n = (int) nums.size();
        st.resize(n);
        path.resize(n);
        sort(nums.begin(), nums.end());
        dfs(nums, 0);
        return res;
    }
};
```