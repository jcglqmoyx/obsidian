## [LeetCode 1986. Minimum Number of Work Sessions to Finish the Tasks](https://leetcode.cn/problems/minimum-number-of-work-sessions-to-finish-the-tasks/)
---
There are `n` tasks assigned to you. The task times are represented as an integer array `tasks` of length `n`, where the `ith` task takes `tasks[i]` hours to finish. A **work session** is when you work for **at most** `sessionTime` consecutive hours and then take a break.

You should finish the given tasks in a way that satisfies the following conditions:

-   If you start a task in a work session, you must complete it in the **same** work session.
-   You can start a new task **immediately** after finishing the previous one.
-   You may complete the tasks in **any order**.

Given `tasks` and `sessionTime`, return *the **minimum** number of **work sessions** needed to finish all the tasks following the conditions above.*

The tests are generated such that `sessionTime` is **greater** than or **equal** to the **maximum** element in `tasks[i]`.

**Example 1:**

**Input:** tasks = \[1,2,3\], sessionTime = 3
**Output:** 2
**Explanation:** You can finish the tasks in two work sessions.
- First work session: finish the first and the second tasks in 1 + 2 = 3 hours.
- Second work session: finish the third task in 3 hours.

**Example 2:**

**Input:** tasks = \[3,1,3,1,1\], sessionTime = 8
**Output:** 2
**Explanation:** You can finish the tasks in two work sessions.
- First work session: finish all the tasks except the last one in 3 + 1 + 3 + 1 = 8 hours.
- Second work session: finish the last task in 1 hour.

**Example 3:**

**Input:** tasks = \[1,2,3,4,5\], sessionTime = 15
**Output:** 1
**Explanation:** You can finish all the tasks in one work session.

**Constraints:**

-   `n == tasks.length`
-   `1 <= n <= 14`
-   `1 <= tasks[i] <= 10`
-   `max(tasks[i]) <= sessionTime <= 15`
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int minSessions(vector<int> &tasks, int sessionTime) {
        int n = (int) tasks.size();
        vector<pair<int, int>> f(1 << n, {INT_MAX, INT_MAX});
        f[0] = {1, 0};

        auto add = [&](const pair<int, int> &o, int x) -> pair<int, int> {
            if (o.second + x <= sessionTime) {
                return {o.first, o.second + x};
            }
            return {o.first + 1, x};
        };

        for (int mask = 1; mask < (1 << n); ++mask) {
            for (int i = 0; i < n; ++i) {
                if (mask & (1 << i)) {
                    f[mask] = min(f[mask], add(f[mask ^ (1 << i)], tasks[i]));
                }
            }
        }
        return f[(1 << n) - 1].first;
    }
};
```