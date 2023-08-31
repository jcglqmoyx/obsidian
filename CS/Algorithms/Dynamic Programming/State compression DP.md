## [LeetCode 1125. Smallest Sufficient Team](https://leetcode.cn/problems/smallest-sufficient-team/description/)
In a project, you have a list of required skills `req_skills`, and a list of people. The `ith` person `people[i]` contains a list of skills that the person has.

Consider a sufficient team: a set of people such that for every required skill in `req_skills`, there is at least one person in the team who has that skill. We can represent these teams by the index of each person.

-   For example, `team = [0, 1, 3]` represents the people with skills `people[0]`, `people[1]`, and `people[3]`.

Return _any sufficient team of the smallest possible size, represented by the index of each person_. You may return the answer in **any order**.

It is **guaranteed** an answer exists.

**Example 1:**

**Input:** req_skills = ["java","nodejs","reactjs"], people = [["java"],["nodejs"],["nodejs","reactjs"]]
**Output:** [0,2]

**Example 2:**

**Input:** req_skills = ["algorithms","math","java","reactjs","csharp","aws"], people = [["algorithms","math","java"],["algorithms","math","reactjs"],["java","csharp","aws"],["reactjs","csharp"],["csharp","math"],["aws","java"]]
**Output:** [1,2]

**Constraints:**

-   `1 <= req_skills.length <= 16`
-   `1 <= req_skills[i].length <= 16`
-   `req_skills[i]` consists of lowercase English letters.
-   All the strings of `req_skills` are **unique**.
-   `1 <= people.length <= 60`
-   `0 <= people[i].length <= 16`
-   `1 <= people[i][j].length <= 16`
-   `people[i][j]` consists of lowercase English letters.
-   All the strings of `people[i]` are **unique**.
-   Every skill in `people[i]` is a skill in `req_skills`.
-   It is guaranteed a sufficient team exists.
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    vector<int> smallestSufficientTeam(vector<string> &req_skills, vector<vector<string>> &people) {  
        int n = (int) req_skills.size(), m = (int) people.size();  
        unordered_map<string, int> skill_map;  
        vector<int> p(m);  
        for (int i = 0; i < n; i++) {  
            skill_map[req_skills[i]] = i;  
        }  
        for (int i = 0; i < m; i++) {  
            for (auto &skill: people[i]) {  
                p[i] |= 1 << skill_map[skill];  
            }  
        }  
        vector<pair<int, int>> path(1 << n);  
        vector<int> f(1 << n, n + 1);  
        f[0] = 0;  
        for (int i = 0; i < 1 << n; i++) {  
            for (int j = 0; j < m; j++) {  
                int v = i | p[j];  
                if (f[v] > f[i] + 1) {  
                    f[v] = f[i] + 1;  
                    path[v] = {i, j};  
                }  
            }  
        }  
        vector<int> res;  
        for (int state = (1 << n) - 1; state;) {  
            res.push_back(path[state].second);  
            state = path[state].first;  
        }  
        return res;  
    }  
};
```
## [LeetCode 1434. Number of Ways to Wear Different Hats to Each Other](https://leetcode.cn/problems/number-of-ways-to-wear-different-hats-to-each-other/description/)
There are `n` people and `40` types of hats labeled from `1` to `40`.

Given a 2D integer array `hats`, where `hats[i]` is a list of all hats preferred by the `ith` person.

Return _the number of ways that the `n` people wear different hats to each other_.

Since the answer may be too large, return it modulo `109 + 7`.

**Example 1:**

**Input:** hats = [[3,4],[4,5],[5]]
**Output:** 1
**Explanation:** There is only one way to choose hats given the conditions. 
First person choose hat 3, Second person choose hat 4 and last one hat 5.

**Example 2:**

**Input:** hats = [[3,5,1],[3,5]]
**Output:** 4
**Explanation:** There are 4 ways to choose hats:
(3,5), (5,3), (1,3) and (1,5)

**Example 3:**

**Input:** hats = [[1,2,3,4],[1,2,3,4],[1,2,3,4],[1,2,3,4]]
**Output:** 24
**Explanation:** Each person can choose hats labeled from 1 to 4.
Number of Permutations of (1,2,3,4) = 24.

**Constraints:**

-   `n == hats.length`
-   `1 <= n <= 10`
-   `1 <= hats[i].length <= 40`
-   `1 <= hats[i][j] <= 40`
-   `hats[i]` contains a list of **unique** integers.

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int numberWays(vector<vector<int>> &hats) {  
        const int MOD = 1e9 + 7;  
        int n = (int) hats.size();  
  
        vector<long long> memo(n);  
        int max_id = 0;  
        for (int i = 0; i < n; i++) {  
            long long mask = 0;  
            for (int hat: hats[i]) {  
                mask |= 1LL << hat;  
                max_id = max(max_id, hat);  
            }  
            memo[i] = mask;  
        }  
  
        int f[max_id + 1][1 << n];  
        memset(f, 0, sizeof f);  
        f[0][0] = 1;  
        for (int i = 1; i <= max_id; i++) {  
            for (int j = 0; j < 1 << n; j++) {  
                f[i][j] = f[i - 1][j];  
                for (int k = 0; k < n; k++) {  
                    if (j >> k & 1 && memo[k] >> i & 1) {  
                        f[i][j] += f[i - 1][j - (1 << k)];  
                        f[i][j] %= MOD;  
                    }  
                }  
            }  
        }  
        return f[max_id][(1 << n) - 1];  
    }  
};
```
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
## [LeetCode 1595. Minimum Cost to Connect Two Groups of Points](https://leetcode.cn/problems/minimum-cost-to-connect-two-groups-of-points/)
---
You are given two groups of points where the first group has `size1` points, the second group has `size2` points, and `size1 >= size2`.

The `cost` of the connection between any two points are given in an `size1 x size2` matrix where `cost[i][j]` is the cost of connecting point `i` of the first group and point `j` of the second group. The groups are connected if **each point in both groups is connected to one or more points in the opposite group**. In other words, each point in the first group must be connected to at least one point in the second group, and each point in the second group must be connected to at least one point in the first group.

Return *the minimum cost it takes to connect the two groups*.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/03/ex1.jpg)

**Input:** cost = \[\[15, 96\], \[36, 2\]\]
**Output:** 17
**Explanation**: The optimal way of connecting the groups is:
1--A
2--B
This results in a total cost of 17.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/03/ex2.jpg)

**Input:** cost = \[\[1, 3, 5\], \[4, 1, 1\], \[1, 5, 3\]\]
**Output:** 4
**Explanation**: The optimal way of connecting the groups is:
1--A
2--B
2--C
3--A
This results in a total cost of 4.
Note that there are multiple points connected to point 2 in the first group and point A in the second group. This does not matter as there is no limit to the number of points that can be connected. We only care about the minimum total cost.

**Example 3:**

**Input:** cost = \[\[2, 5, 1\], \[3, 4, 7\], \[8, 1, 2\], \[6, 2, 4\], \[3, 8, 8\]\]
**Output:** 10

**Constraints:**

-   `size1 == cost.length`
-   `size2 == cost[i].length`
-   `1 <= size1, size2 <= 12`
-   `size1 >= size2`
-   `0 <= cost[i][j] <= 100`
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int connectTwoGroups(vector<vector<int>> &cost) {
        int n = (int) cost.size(), m = (int) cost[0].size();
        vector<vector<int>> prices(n, vector<int>(1 << m));
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < 1 << m; j++) {
                for (int k = 0; k < m; k++) {
                    if (j >> k & 1) {
                        prices[i][j] += cost[i][k];
                    }
                }
            }
        }
        vector<vector<int>> f(n, vector<int>(1 << m, 1e8));
        for (int j = 0; j < 1 << m; j++) {
            f[0][j] = prices[0][j];
        }
        for (int i = 1; i < n; i++) {
            f[i][0] = 0;
            for (int j = 0; j < 1 << m; j++) {
                for (int k = (j - 1) & j; k; k = (k - 1) & j) {
                    f[i][j] = min(f[i][j], f[i - 1][j ^ k] + prices[i][k]);
                }
                for (int k = 0; k < m; k++) {
                    if (j >> k & 1) {
                        f[i][j] = min(f[i][j], f[i - 1][j] + cost[i][k]);
                    }
                }
            }
        }
        return f[n - 1][(1 << m) - 1];
    }
};
```
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
## [LeetCode 1723. Find Minimum Time to Finish All Jobs](https://leetcode.com/problems/find-minimum-time-to-finish-all-jobs/description/)
You are given an integer array `jobs`, where `jobs[i]` is the amount of time it takes to complete the `ith` job.

There are `k` workers that you can assign jobs to. Each job should be assigned to **exactly** one worker. The **working time** of a worker is the sum of the time it takes to complete all jobs assigned to them. Your goal is to devise an optimal assignment such that the **maximum working time** of any worker is **minimized**.

_Return the **minimum** possible **maximum working time** of any assignment._

**Example 1:**

**Input:** jobs = [3,2,3], k = 3
**Output:** 3
**Explanation:** By assigning each person one job, the maximum time is 3.

**Example 2:**

**Input:** jobs = [1,2,4,7,8], k = 2
**Output:** 11
**Explanation:** Assign the jobs the following way:
Worker 1: 1, 2, 8 (working time = 1 + 2 + 8 = 11)
Worker 2: 4, 7 (working time = 4 + 7 = 11)
The maximum working time is 11.

**Constraints:**

1 <= k <= jobs.length <= 12
1 <= jobs[i] <= $10^7$

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int minimumTimeRequired(vector<int> &jobs, int k) {  
        int n = (int) jobs.size();  
  
        int sum[1 << n];  
        memset(sum, 0, sizeof sum);  
        for (int i = 1; i < 1 << n; i++) {  
            int j = i & (i - 1);  
            sum[i] = sum[j] + jobs[__builtin_ctz(i ^ j)];  
        }  
  
        int f[k][1 << n];  
        memset(f, 0x3f, sizeof f);  
        for (int i = 0; i < 1 << n; i++) {  
            f[0][i] = sum[i];  
        }  
        for (int i = 1; i < k; i++) {  
            for (int j = 0; j < 1 << n; j++) {  
                for (int u = j; u; u = (u - 1) & j) {  
                    f[i][j] = min(f[i][j], max(f[i - 1][j - u], sum[u]));  
                }  
            }  
        }  
  
        return f[k - 1][(1 << n) - 1];  
    }  
};
```
## [LeetCoded 1799. Maximize Score After N Operations](https://leetcode.cn/problems/maximize-score-after-n-operations/description/)
You are given `nums`, an array of positive integers of size `2 * n`. You must perform `n` operations on this array.

In the `ith` operation **(1-indexed)**, you will:

-   Choose two elements, `x` and `y`.
-   Receive a score of `i * gcd(x, y)`.
-   Remove `x` and `y` from `nums`.

Return _the maximum score you can receive after performing_ `n` _operations._

The function `gcd(x, y)` is the greatest common divisor of `x` and `y`.

---

**Example 1:**

**Input:** nums = [1,2]
**Output:** 1
**Explanation:** The optimal choice of operations is:
(1 * gcd(1, 2)) = 1

**Example 2:**

**Input:** nums = [3,4,6,8]
**Output:** 11
**Explanation:** The optimal choice of operations is:
(1 * gcd(3, 6)) + (2 * gcd(4, 8)) = 3 + 8 = 11

**Example 3:**

**Input:** nums = [1,2,3,4,5,6]
**Output:** 14
**Explanation:** The optimal choice of operations is:
(1 * gcd(1, 5)) + (2 * gcd(2, 4)) + (3 * gcd(3, 6)) = 1 + 4 + 9 = 14

**Constraints:**
1 <= n <= 7
nums.length == 2 * n
1 <= nums[i] <= $10^6$

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int maxScore(vector<int> &nums) {  
        int n = (int) nums.size();  
        int f[1 << n], gcd[n][n];  
        memset(f, 0, sizeof f);  
        for (int i = 0; i < n; i++) {  
            for (int j = i + 1; j < n; j++) {  
                gcd[i][j] = __gcd(nums[i], nums[j]);  
            }  
        }  
        for (int i = 0; i < 1 << n; i++) {  
            int cnt = __builtin_popcount(i);  
            if (!(cnt & 1)) {  
                for (int j = 0; j < n; j++) {  
                    if (i >> j & 1) {  
                        for (int k = j + 1; k < n; k++) {  
                            if (i >> k & 1) {  
                                f[i] = max(f[i], f[i ^ (1 << j) ^ (1 << k)] + ((n - cnt) / 2 + 1) * gcd[j][k]);  
                            }  
                        }  
                    }  
                }  
            }  
        }  
        return f[(1 << n) - 1];  
    }  
};
```

## [LeetCode 1879. Minimum XOR Sum of Two Arrays](https://leetcode.cn/problems/minimum-xor-sum-of-two-arrays/)
---
You are given two integer arrays `nums1` and `nums2` of length `n`.

The **XOR sum** of the two integer arrays is `(nums1[0] XOR nums2[0]) + (nums1[1] XOR nums2[1]) + ... + (nums1[n - 1] XOR nums2[n - 1])` (**0-indexed**).

-   For example, the **XOR sum** of `[1,2,3]` and `[3,2,1]` is equal to `(1 XOR 3) + (2 XOR 2) + (3 XOR 1) = 2 + 0 + 2 = 4`.

Rearrange the elements of `nums2` such that the resulting **XOR sum** is **minimized**.

Return *the **XOR sum** after the rearrangement*.

**Example 1:**

**Input:** nums1 = \[1,2\], nums2 = \[2,3\]
**Output:** 2
**Explanation:** Rearrange `nums2` so that it becomes `[3,2]`.
The XOR sum is (1 XOR 3) + (2 XOR 2) = 2 + 0 = 2.

**Example 2:**

**Input:** nums1 = \[1,0,3\], nums2 = \[5,3,4\]
**Output:** 8
**Explanation:** Rearrange `nums2` so that it becomes `[5,4,3]`. 
The XOR sum is (1 XOR 5) + (0 XOR 4) + (3 XOR 3) = 4 + 4 + 0 = 8.

**Constraints:**

-   `n == nums1.length`
-   `n == nums2.length`
-   `1 <= n <= 14`
-   `0 <= nums1[i], nums2[i] <= 107`
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int minimumXORSum(vector<int> &nums1, vector<int> &nums2) {  
        int n = (int) nums1.size(), INF = 1e9;  
        vector<int> f(1 << n, INF);  
        f[0] = 0;  
        for (int i = 1; i < 1 << n; i++) {  
            int s = 0;  
            for (int j = 0; j < n; j++) {  
                if (i >> j & 1) s++;  
            }  
            for (int j = 0; j < n; j++) {  
                if (i >> j & 1) {  
                    f[i] = min(f[i], f[i - (1 << j)] + (nums1[s - 1] ^ nums2[j]));  
                }  
            }  
        }  
        return f[(1 << n) - 1];  
    }  
};
```
## [LeetCode 1947. Maximum Compatibility Score Sum](https://leetcode.cn/problems/maximum-compatibility-score-sum/)
There is a survey that consists of `n` questions where each question's answer is either `0` (no) or `1` (yes).

The survey was given to `m` students numbered from `0` to `m - 1` and `m` mentors numbered from `0` to `m - 1`. The answers of the students are represented by a 2D integer array `students` where `students[i]` is an integer array that contains the answers of the `ith` student (**0-indexed**). The answers of the mentors are represented by a 2D integer array `mentors` where `mentors[j]` is an integer array that contains the answers of the `jth` mentor (**0-indexed**).

Each student will be assigned to **one** mentor, and each mentor will have **one** student assigned to them. The **compatibility score** of a student-mentor pair is the number of answers that are the same for both the student and the mentor.

-   For example, if the student's answers were `[1, 0, 1]` and the mentor's answers were `[0, 0, 1]`, then their compatibility score is 2 because only the second and the third answers are the same.

You are tasked with finding the optimal student-mentor pairings to **maximize** the **sum of the compatibility scores**.

Given `students` and `mentors`, return *the **maximum compatibility score sum** that can be achieved.*

**Example 1:**

**Input:** students = \[\[1,1,0\],\[1,0,1\],\[0,0,1\]\], mentors = \[\[1,0,0\],\[0,0,1\],\[1,1,0\]\]
**Output:** 8
**Explanation:** We assign students to mentors in the following way:
- student 0 to mentor 2 with a compatibility score of 3.
- student 1 to mentor 0 with a compatibility score of 2.
- student 2 to mentor 1 with a compatibility score of 3.
The compatibility score sum is 3 + 2 + 3 = 8.

**Example 2:**

**Input:** students = \[\[0,0\],\[0,0\],\[0,0\]\], mentors = \[\[1,1\],\[1,1\],\[1,1\]\]
**Output:** 0
**Explanation:** The compatibility score of any student-mentor pair is 0.

**Constraints:**

-   `m == students.length == mentors.length`
-   `n == students[i].length == mentors[j].length`
-   `1 <= m, n <= 8`
-   `students[i][k]` is either `0` or `1`.
-   `mentors[j][k]` is either `0` or `1`.
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int maxCompatibilitySum(vector<vector<int>> &students, vector<vector<int>> &mentors) {  
        int n = (int) students.size(), m = (int) students[0].size();  
        int vs[n], vm[n];  
        memset(vs, 0, sizeof vs);  
        memset(vm, 0, sizeof vm);  
        for (int i = 0; i < n; i++) {  
            int mask = 0;  
            for (int j = 0; j < m; j++) {  
                if (students[i][j]) mask |= 1 << j;  
            }  
            vs[i] = mask;  
        }  
        for (int i = 0; i < n; i++) {  
            int mask = 0;  
            for (int j = 0; j < m; j++) {  
                if (mentors[i][j]) mask |= 1 << j;  
            }  
            vm[i] = mask;  
        }  
        int memo[n][n];  
        for (int i = 0; i < n; i++) {  
            for (int j = 0; j < n; j++) {  
                memo[i][j] = __builtin_popcount(~(vs[i] ^ vm[j])) - (32 - m);  
            }  
        }  
        int f[1 << n];  
        memset(f, 0, sizeof f);  
        for (int i = 1; i < 1 << n; i++) {  
            int c = __builtin_popcount(i);  
            for (int j = 0; j < n; j++) {  
                if (i >> j & 1) {  
                    f[i] = max(f[i], f[i ^ (1 << j)] + memo[c - 1][j]);  
                }  
            }  
        }  
        return f[(1 << n) - 1];  
    }  
};
```
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
## [LeetCode 2172. Maximum AND Sum of Array](https://leetcode.cn/problems/maximum-and-sum-of-array/)
---
You are given an integer array `nums` of length `n` and an integer `numSlots` such that `2 * numSlots >= n`. There are `numSlots` slots numbered from `1` to `numSlots`.

You have to place all `n` integers into the slots such that each slot contains at **most** two numbers. The **AND sum** of a given placement is the sum of the **bitwise** `AND` of every number with its respective slot number.

-   For example, the **AND sum** of placing the numbers `[1, 3]` into slot `1` and `[4, 6]` into slot `2` is equal to `(1 AND 1) + (3 AND 1) + (4 AND 2) + (6 AND 2) = 1 + 1 + 0 + 2 = 4`.

Return *the maximum possible **AND sum** of* `nums` *given* `numSlots` *slots.*

**Example 1:**

**Input:** nums = \[1,2,3,4,5,6\], numSlots = 3
**Output:** 9
**Explanation:** One possible placement is \[1, 4\] into slot 1, \[2, 6\] into slot 2, and \[3, 5\] into slot 3. 
This gives the maximum AND sum of (1 AND 1) + (4 AND 1) + (2 AND 2) + (6 AND 2) + (3 AND 3) + (5 AND 3) = 1 + 0 + 2 + 2 + 3 + 1 = 9.

**Example 2:**

**Input:** nums = \[1,3,10,4,7,1\], numSlots = 9
**Output:** 24
**Explanation:** One possible placement is \[1, 1\] into slot 1, \[3\] into slot 3, \[4\] into slot 4, \[7\] into slot 7, and \[10\] into slot 9.
This gives the maximum AND sum of (1 AND 1) + (1 AND 1) + (3 AND 3) + (4 AND 4) + (7 AND 7) + (10 AND 9) = 1 + 1 + 3 + 4 + 7 + 8 = 24.
Note that slots 2, 5, 6, and 8 are empty which is permitted.

**Constraints:**

-   `n == nums.length`
-   `1 <= numSlots <= 9`
-   `1 <= n <= 2 * numSlots`
-   `1 <= nums[i] <= 15`
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int maximumANDSum(vector<int> &nums, int numSlots) {
        int tot = (int) pow(3, numSlots);
        vector<int> f(tot);
        for (int mask = 1; mask < tot; mask++) {
            int cnt = 0;
            for (int dummy = mask, i = 0; i < numSlots; i++, dummy /= 3) {
                cnt += dummy % 3;
            }
            if (cnt > nums.size()) continue;
            for (int dummy = mask, w = 1, i = 1; i <= numSlots; i++, w *= 3, dummy /= 3) {
                int has = dummy % 3;
                if (has) {
                    f[mask] = max(f[mask], f[mask - w] + (nums[cnt - 1] & i));
                }
            }
        }
        return *max_element(f.begin(), f.end());
    }
};
```
## [LeetCode 2572. Count the Number of Square-Free Subsets](https://leetcode.cn/problems/count-the-number-of-square-free-subsets/)
---
You are given a positive integer **0-indexed** array `nums`.

A subset of the array `nums` is **square-free** if the product of its elements is a **square-free integer**.

A **square-free integer** is an integer that is divisible by no square number other than `1`.

Return *the number of square-free non-empty subsets of the array* **nums**. Since the answer may be too large, return it **modulo** `109 + 7`.

A **non-empty** **subset** of `nums` is an array that can be obtained by deleting some (possibly none but not all) elements from `nums`. Two subsets are different if and only if the chosen indices to delete are different.

**Example 1:**

**Input:** nums = \[3,4,4,5\]
**Output:** 3
**Explanation:** There are 3 square-free subsets in this example:
- The subset consisting of the 0th element \[3\]. The product of its elements is 3, which is a square-free integer.
- The subset consisting of the 3rd element \[5\]. The product of its elements is 5, which is a square-free integer.
- The subset consisting of 0th and 3rd elements \[3,5\]. The product of its elements is 15, which is a square-free integer.
It can be proven that there are no more than 3 square-free subsets in the given array.

**Example 2:**

**Input:** nums = \[1\]
**Output:** 1
**Explanation:** There is 1 square-free subset in this example:
- The subset consisting of the 0th element \[1\]. The product of its elements is 1, which is a square-free integer.
It can be proven that there is no more than 1 square-free subset in the given array.

**Constraints:**

-   `1 <= nums.length <= 1000`
-   `1 <= nums[i] <= 30`

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int squareFreeSubsets(vector<int> &nums) {  
        const int MOD = 1e9 + 7;  
        const int N_PRIMES = 10, MX = 30, M = 1 << N_PRIMES;  
        int primes[N_PRIMES] = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29};  
        int mask[31]{};  
        for (int i = 2; i <= MX; i++) {  
            for (int j = 0; j < N_PRIMES; j++) {  
                int x = primes[j];  
                if (i % x == 0) {  
                    if (i % (x * x) == 0) {  
                        mask[i] = -1;  
                        break;                    }  
                    mask[i] |= 1 << j;  
                }  
            }  
        }  
        int f[M]{1};  
        for (int x: nums) {  
            if (mask[x] == -1) continue;  
            for (int j = M - 1; j >= mask[x]; j--) {  
                if ((mask[x] | j) == j) f[j] = (f[j] + f[j ^ mask[x]]) % MOD;  
            }  
        }  
        int res = 0;  
        for (int i: f) {  
            res = (res + i) % MOD;  
        }  
        return (res - 1 + MOD) % MOD;  
    }  
};
```