## [LeetCode **1124. Longest Well-Performing Interval**](https://leetcode.cn/problems/longest-well-performing-interval/)

We are given `hours`, a list of the number of hours worked per day for a given employee.

A day is considered to be a _tiring day_ if and only if the number of hours worked is (strictly) greater than `8`.

A _well-performing interval_ is an interval of days for which the number of tiring days is strictly larger than the number of non-tiring days.

Return the length of the longest well-performing interval.

**Example 1:**

```
Input: hours = [9,9,6,0,6,6,9]
Output: 3
Explanation:The longest well-performing interval is [9,9,6].
```

**Example 2:**

```
Input: hours = [6,6,6]
Output: 0
```

**Constraints:**

-   `1 <= hours.length <= 10000`
-   `0 <= hours[i] <= 16`

### Greedy

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int longestWPI(vector<int> &hours) {
        int n = (int) hours.size();
        vector<int> s(n + 1);
        stack<int> stk;
        stk.push(0);
        for (int i = 1; i <= n; i++) {
            s[i] = s[i - 1] + (hours[i - 1] > 8 ? 1 : -1);
            if (s[stk.top()] > s[i]) {
                stk.push(i);
            }
        }
        int res = 0;
        for (int r = n; r >= 1; r--) {
            while (!stk.empty() && s[stk.top()] < s[r]) {
                res = max(res, r - stk.top());
                stk.pop();
            }
        }
        return res;
    }
};
```

### Hash table

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int longestWPI(vector<int> &hours) {
        int res = 0;
        unordered_map<int, int> map;
        for (int i = 0, sum = 0; i < hours.size(); i++) {
            sum += hours[i] > 8 ? 1 : -1;
            if (sum > 0) res = i + 1;
            else {
                if (!map.count(sum)) map[sum] = i;
                if (map.count(sum - 1)) res = max(res, i - map[sum - 1]);
            }
        }
        return res;
    }
};
```