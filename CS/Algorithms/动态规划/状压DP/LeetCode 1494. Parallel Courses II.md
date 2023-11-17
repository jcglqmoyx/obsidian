## [LeetCode 1494. Parallel Courses II](https://leetcode.cn/problems/parallel-courses-ii/)
You are given an integer `n`, which indicates that there are `n` courses labeled from `1` to `n`. You are also given an array `relations` where `relations[i] = [prevCoursei, nextCoursei]`, representing a prerequisite relationship between course `prevCoursei` and course `nextCoursei`: course `prevCoursei` has to be taken before course `nextCoursei`. Also, you are given the integer `k`.

In one semester, you can take **at most** `k` courses as long as you have taken all the prerequisites in the **previous** semesters for the courses you are taking.

Return _the **minimum** number of semesters needed to take all courses_. The testcases will be generated such that it is possible to take every course.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/05/22/leetcode_parallel_courses_1.png)

**Input:** n = 4, relations = [[2,1],[3,1],[1,4]], k = 2  
**Output:** 3  
**Explanation:** The figure above represents the given graph.  
In the first semester, you can take courses 2 and 3.  
In the second semester, you can take course 1.  
In the third semester, you can take course 4.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/05/22/leetcode_parallel_courses_2.png)

**Input:** n = 5, relations = [[2,1],[3,1],[4,1],[1,5]], k = 2  
**Output:** 4  
**Explanation:** The figure above represents the given graph.  
In the first semester, you can only take courses 2 and 3 since you cannot take more than two per semester.  
In the second semester, you can take course 4.  
In the third semester, you can take course 1.  
In the fourth semester, you can take course 5.

**Constraints:**

-   `1 <= n <= 15`
-   `1 <= k <= n`
-   `0 <= relations.length <= n * (n-1) / 2`
-   `relations[i].length == 2`
-   `1 <= prevCoursei, nextCoursei <= n`
-   `prevCoursei != nextCoursei`
-   All the pairs `[prevCoursei, nextCoursei]` are **unique**.
-   The given graph is a directed acyclic graph.

memoization dp

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
    int f[1 << 15];  
  
    void dfs(int n, int k, int state, int valid, int start, int selected) {  
        if (!k || !valid) {  
            f[state | selected] = min(f[state | selected], f[state] + 1);  
        } else {  
            for (int i = start; i < n; i++) {  
                if (valid >> i & 1) {  
                    dfs(n, k - 1, state, valid ^ 1 << i, i + 1, selected | 1 << i);  
                }  
            }  
        }  
    }  
  
public:  
    int minNumberOfSemesters(int n, vector<vector<int>> &relations, int k) {  
        memset(f, 0x3f, sizeof f);  
        f[0] = 0;  
        for (auto &r: relations) r[0]--, r[1]--;  
        for (int i = 0; i < 1 << n; i++) {  
            int invalid = 0;  
            for (auto &r: relations) {  
                if (!(i >> r[0] & 1)) invalid |= 1 << r[1];  
            }  
            int valid = 0;  
            for (int j = 0; j < n; j++) {  
                if (!(i >> j & 1) && !(invalid >> j & 1)) valid |= 1 << j;  
            }  
            dfs(n, k, i, valid, 0, 0);  
        }  
        return f[(1 << n) - 1];  
    }  
};
```

dp
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int minNumberOfSemesters(int n, vector<vector<int>> &relations, int k) {  
        int pre[n];  
        memset(pre, 0, sizeof pre);  
        for (auto &r: relations) {  
            pre[r[1] - 1] |= 1 << (r[0] - 1);  
        }  
        int f[1 << n];  
        memset(f, 0x3f, sizeof f);  
        f[0] = 0;  
        for (int i = 0; i < 1 << n; i++) {  
            int to_study = 0;  
            for (int j = 0; j < n; j++) {  
                if ((pre[j] & i) == pre[j] && !(i >> j & 1)) {  
                    to_study |= 1 << j;  
                }  
            }  
            for (int t = to_study; t; t = (t - 1) & to_study) {  
                if (__builtin_popcount(t) <= k) {  
                    f[i | t] = min(f[i | t], f[i] + 1);  
                }  
            }  
        }  
        return f[(1 << n) - 1];  
    }  
};
```