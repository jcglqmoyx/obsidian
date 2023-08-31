## **[LeetCode 45. Jump Game II](https://leetcode.cn/problems/jump-game-ii/)**

You are given a **0-indexed** array of integers `nums` of length `n`. You are initially positioned at `nums[0]`.

Each element `nums[i]` represents the maximum length of a forward jump from index `i`. In other words, if you are at `nums[i]`, you can jump to any `nums[i + j]` where:

-   `0 <= j <= nums[i]` and
-   `i + j < n`

Return _the minimum number of jumps to reach_ `nums[n - 1]`. The test cases are generated such that you can reach `nums[n - 1]`.

**Example 1:**

```
Input: nums = [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**Example 2:**

```
Input: nums = [2,3,0,1,4]
Output: 2
```

**Constraints:**

-   `1 <= nums.length <= 10000`
-   `0 <= nums[i] <= 1000`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int jump(vector<int> &nums) {
        int n = (int) nums.size(), res = 0, mx = 0, end = 0;
        for (int i = 0; i < n - 1; i++) {
            mx = max(mx, i + nums[i]);
            if (i == end) {
                res++;
                end = mx;
            }
        }
        return res;
    }
};
```

## [LeetCode **630. Course Schedule III**](https://leetcode.cn/problems/course-schedule-iii/)

There are `n` different online courses numbered from `1` to `n`. You are given an array `courses` where `courses[i] = [$duration_i$, $lastDay_i$]` indicate that the$`i^{th}`$ course should be taken **continuously** for$`duration_i`$ days and must be finished before or on$`lastDay_i`$.

You will start on the$`1^{st}`$ day and you cannot take two or more courses simultaneously.

Return _the maximum number of courses that you can take_.

**Example 1:**

```
Input: courses = [[100,200],[200,1300],[1000,1250],[2000,3200]]
Output: 3
Explanation:
There are totally 4 courses, but you can take 3 courses at most:
First, take the 1st course, it costs 100 days so you will finish it on the 100th day, and ready to take the next course on the 101st day.
Second, take the 3rd course, it costs 1000 days so you will finish it on the 1100th day, and ready to take the next course on the 1101st day.
Third, take the 2nd course, it costs 200 days so you will finish it on the 1300th day.
The 4th course cannot be taken now, since you will finish it on the 3300th day, which exceeds the closed date.
```

**Example 2:**

```
Input: courses = [[1,2]]
Output: 1
```

**Example 3:**

```
Input: courses = [[3,2],[4,3]]
Output: 0
```

**Constraints:**

-   `1 <= courses.length <= 10000`
-   `1 <= $duration_i$, $lastDay_i$ <= 10000`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int scheduleCourse(vector<vector<int>> &courses) {
        sort(courses.begin(), courses.end(), [&](const vector<int> &a, const vector<int> &b) {
            return a[1] < b[1];
        });
        priority_queue<int> heap;
        int tot = 0;
        for (auto &course: courses) {
            tot += course[0];
            heap.push(course[0]);
            if (tot > course[1]) {
                int t = heap.top();
                tot -= t;
                heap.pop();
            }
        }
        return (int) heap.size();
    }
};
```

## [LeetCode **857. Minimum Cost to Hire K Workers**](https://leetcode.com/problems/minimum-cost-to-hire-k-workers/)

There are `n` workers. You are given two integer arrays `quality` and `wage` where `quality[i]` is the quality of the `ith` worker and `wage[i]` is the minimum wage expectation for the $`i^{th}`$ worker.

We want to hire exactly `k` workers to form a paid group. To hire a group of `k` workers, we must pay them according to the following rules:

1.  Every worker in the paid group should be paid in the ratio of their quality compared to other workers in the paid group.
2.  Every worker in the paid group must be paid at least their minimum wage expectation.

Given the integer `k`, return _the least amount of money needed to form a paid group satisfying the above conditions_. Answers within $`10^{-5}`$ of the actual answer will be accepted.

**Example 1:**

```
Input: quality = [10,20,5], wage = [70,50,30], k = 2
Output: 105.00000
Explanation: We pay 70 to 0th worker and 35 to 2nd worker.
```

**Example 2:**

```
Input: quality = [3,1,10,10,1], wage = [4,8,2,2,7], k = 3
Output: 30.66667
Explanation: We pay 4 to 0th worker, 13.33333 to 2nd and 3rd workers separately.
```

**Constraints:**

-   `n == quality.length == wage.length`
-   `1 <= k <= n <= 10000`
-   `1 <= quality[i], wage[i] <= 10000`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    double mincostToHireWorkers(vector<int> &quality, vector<int> &wage, int k) {
        using PII = pair<int, int>;
        int n = (int) quality.size();
        vector<PII> v(n);
        for (int i = 0; i < n; i++) {
            v[i] = {quality[i], wage[i]};
        }
        sort(v.begin(), v.end(), [](const PII &a, const PII &b) {
            return a.second * b.first < b.second * a.first;
        });
        int sq = 0;
        double res = 1e18;
        priority_queue<int> heap;
        for (int i = 0; i < n; i++) {
            sq += v[i].first;
            double w = sq * (double) v[i].second / v[i].first;
            heap.push(v[i].first);
            if (i >= k - 1) {
                res = min(res, w);
                auto t = heap.top();
                heap.pop();
                sq -= t;
            }
        }
        return res;
    }
};
```

## [LeetCode 1053. Previous Permutation With One Swap](https://leetcode.cn/problems/previous-permutation-with-one-swap/)
Given an array of positive integers `arr` (not necessarily distinct), return _the_ lexicographically _largest permutation that is smaller than_ `arr`, that can be **made with exactly one swap**. If it cannot be done, then return the same array.

**Note** that a _swap_ exchanges the positions of two numbers `arr[i]` and `arr[j]`

**Example 1:**
```
Input: arr = [3,2,1]
Output: [3,1,2]
Explanation: Swapping 2 and 1.
```

**Example 2:**
```
Input: arr = [1,1,5]
Output: [1,1,5]
Explanation: This is already the smallest permutation.
```

**Example 3:**
```
Input: arr = [1,9,4,6,7]
Output: [1,7,4,6,9]
Explanation: Swapping 9 and 7.
```

**Constraints:**
1 <= arr.length <= $10^4$
1 <= arr[i] <= $10^4$
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    vector<int> prevPermOpt1(vector<int> &arr) {  
        int n = (int) arr.size();  
        for (int i = n - 2; i >= 0; i--) {  
            if (arr[i] > arr[i + 1]) {  
                int j = n - 1;  
                while (arr[j] >= arr[i] || arr[j] == arr[j - 1]) j--;  
                swap(arr[i], arr[j]);  
                break;            }  
        }  
        return arr;  
    }  
};
```
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

## [LeetCode **1353. Maximum Number of Events That Can Be Attended**](https://leetcode.cn/problems/maximum-number-of-events-that-can-be-attended/)

You are given an array of `events` where `events[i] = [$startDay_i$, $endDay_i$]`. Every event `i` starts at$`startDay_i`$ and ends at$`endDayi`$.

You can attend an event `i` at any day `d` where$`startTime_i$ <= d <= $endTime_i$`. You can only attend one event at any time `d`.

Return _the maximum number of events you can attend_.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/02/05/e1.png](https://assets.leetcode.com/uploads/2020/02/05/e1.png)

```
Input: events = [[1,2],[2,3],[3,4]]
Output: 3
Explanation: You can attend all the three events.
One way to attend them all is as shown.
Attend the first event on day 1.
Attend the second event on day 2.
Attend the third event on day 3.
```

**Example 2:**

```
Input: events= [[1,2],[2,3],[3,4],[1,2]]
Output: 4
```

**Constraints:**

-   `1 <= events.length <= $10^5$`
-   `events[i].length == 2`
-   `1 <= $startDay_i$ <= $endDay_i$ <= $10^5$`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int maxEvents(vector<vector<int>> &events) {
        sort(events.begin(), events.end());
        priority_queue<int, vector<int>, greater<>> heap;
        int n = (int) events.size(), i = 0, res = 0, day = 1;
        while (i < n || !heap.empty()) {
            while (i < n && events[i][0] == day) {
                heap.push(events[i][1]);
                i++;
            }
            while (!heap.empty() && heap.top() < day) {
                heap.pop();
            }
            if (!heap.empty()) {
                heap.pop();
                res++;
            }
            day++;
        }
        return res;
    }
};
```

## [LeetCode **1383. Maximum Performance of a Team**](https://leetcode.cn/problems/maximum-performance-of-a-team/description/)

You are given two integers `n` and `k` and two integer arrays `speed` and `efficiency` both of length `n`. There are `n` engineers numbered from `1` to `n`. `speed[i]` and `efficiency[i]` represent the speed and efficiency of the `ith` engineer respectively.

Choose **at most** `k` different engineers out of the `n` engineers to form a team with the maximum **performance**.

The performance of a team is the sum of their engineers' speeds multiplied by the minimum efficiency among their engineers.

Return _the maximum performance of this team_. Since the answer can be a huge number, return it **modulo** $10^9+7$.

**Example 1:**

```
Input: n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 2
Output: 60
Explanation:
We have the maximum performance of the team by selecting engineer 2 (with speed=10 and efficiency=4) and engineer 5 (with speed=5 and efficiency=7). That is, performance = (10 + 5) * min(4, 7) = 60.
```

**Example 2:**

```
Input: n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 3
Output: 68
Explanation:
This is the same example as the first but k = 3. We can select engineer 1, engineer 2 and engineer 5 to get the maximum performance of the team. That is, performance = (2 + 10 + 5) * min(5, 4, 7) = 68.
```

**Example 3:**

```
Input: n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 4
Output: 72
```

**Constraints:**

1 <= k <= n <= $10^5$
speed.length == n
efficiency.length == n
1 <= speed[i] <= $10^5$
1 <= efficiency[i] <= $10^8$

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int maxPerformance(int n, vector<int> &speed, vector<int> &efficiency, int k) {
        using PII = pair<int, int>;
        const int MOD = 1e9 + 7;
        
        vector<PII> v(n);
        for (int i = 0; i < n; i++) v[i] = {efficiency[i], speed[i]};
        sort(v.begin(), v.end(), [](pair<int, int> &a, pair<int, int> &b) {
            return a.first > b.first;
        });
        priority_queue<int, vector<int>, greater<>> heap;
        long long res = 0, sum = 0;
        for (int i = 0; i < n; i++) {
            sum += v[i].second;
            res = max(res, sum * v[i].first);
            heap.push(v[i].second);
            if (i >= k - 1) {
                sum -= heap.top();
                heap.pop();
            }
        }
        return (int) (res % MOD);
    }
};
```

## [LeetCode **1526. Minimum Number of Increments on Subarrays to Form a Target Array**](https://leetcode.cn/problems/minimum-number-of-increments-on-subarrays-to-form-a-target-array/description/)

You are given an integer array `target`. You have an integer array `initial` of the same size as `target` with all elements initially zeros.

In one operation you can choose **any** subarray from `initial` and increment each value by one.

Return _the minimum number of operations to form a_ `target` _array from_ `initial`.

The test cases are generated so that the answer fits in a 32-bit integer.

**Example 1:**

```
Input: target = [1,2,3,2,1]
Output: 3
Explanation: We need at least 3 operations to form the target array from the initial array.
[0,0,0,0,0] increment 1 from index 0 to 4 (inclusive).
[1,1,1,1,1] increment 1 from index 1 to 3 (inclusive).
[1,2,2,2,1] increment 1 at index 2.
[1,2,3,2,1] target array is formed.
```

**Example 2:**

```
Input: target = [3,1,1,2]
Output: 4
Explanation: [0,0,0,0] -> [1,1,1,1] -> [1,1,1,2] -> [2,1,1,2] -> [3,1,1,2]
```

**Example 3:**

```
Input: target = [3,1,5,4,2]
Output: 7
Explanation: [0,0,0,0,0] -> [1,1,1,1,1] -> [2,1,1,1,1] -> [3,1,1,1,1] -> [3,1,2,2,2] -> [3,1,3,3,2] -> [3,1,4,4,2] -> [3,1,5,4,2].
```

**Constraints:**

-   `1 <= target.length <= $10^5$`
-   `1 <= target[i] <= $10^5$`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int minNumberOperations(vector<int> &target) {
        int ans = target[0];
        for (int i = 0; i < (int) target.size() - 1; i++) {
            if (target[i + 1] > target[i]) {
                ans += target[i + 1] - target[i];
            }
        }
        return ans;
    }
};
```

## [LeetCode **1675. Minimize Deviation in Array**](https://leetcode.cn/problems/minimize-deviation-in-array/description/)

You are given an array `nums` of `n` positive integers.

You can perform two types of operations on any element of the array any number of times:

-   If the element is **even**, **divide** it by `2`.
    -   For example, if the array is `[1,2,3,4]`, then you can do this operation on the last element, and the array will be `[1,2,3,2].`
-   If the element is **odd**, **multiply** it by `2`.
    -   For example, if the array is `[1,2,3,4]`, then you can do this operation on the first element, and the array will be `[2,2,3,4].`

The **deviation** of the array is the **maximum difference** between any two elements in the array.

Return _the **minimum deviation** the array can have after performing some number of operations._

**Example 1:**

```
Input: nums = [1,2,3,4]
Output: 1
Explanation: You can transform the array to [1,2,3,2], then to [2,2,3,2], then the deviation will be 3 - 2 = 1.
```

**Example 2:**

```
Input: nums = [4,1,5,20,3]
Output: 3
Explanation: You can transform the array after two operations to [4,2,5,5,3], then the deviation will be 5 - 2 = 3.
```

**Example 3:**

```
Input: nums = [2,10,8]
Output: 3
```

**Constraints:**

-   `n == nums.length`
-   `2 <= n <= 5 * $10^4$`
-   `1 <= nums[i] <= $10^9$`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int minimumDeviation(vector<int> &nums) {
        priority_queue<int> heap;
        int mn = INT32_MAX;
        for (int num: nums) {
            if (num & 1) num <<= 1;
            heap.push(num);
            mn = min(mn, num);
        }
        int res = INT32_MAX;
        while (true) {
            int x = heap.top();
            heap.pop();
            res = min(res, x - mn);
            if (x & 1) break;
            x >>= 1;
            mn = min(mn, x);
            heap.push(x);
        }
        return res;
    }
};
```

## [LeetCode 1798. Maximum Number of Consecutive Values You Can Make](https://leetcode.cn/problems/maximum-number-of-consecutive-values-you-can-make/description/)

You are given an integer array `coins` of length `n` which represents the `n` coins that you own. The value of the $i^{th}$ coin is `coins[i]`. You can **make** some value `x` if you can choose some of your `n` coins such that their values sum up to `x`.

Return the _maximum number of consecutive integer values that you **can** **make** with your coins **starting** from and **including**_ `0`.

Note that you may have multiple coins of the same value.

**Example 1:**

```
Input: coins = [1,3]
Output: 2
Explanation:You can make the following values:
- 0: take []
- 1: take [1]
You can make 2 consecutive integer values starting from 0.
```

**Example 2:**

```
Input: coins = [1,1,1,4]
Output: 8
Explanation:You can make the following values:
- 0: take []
- 1: take [1]
- 2: take [1,1]
- 3: take [1,1,1]
- 4: take [4]
- 5: take [4,1]
- 6: take [4,1,1]
- 7: take [4,1,1,1]
You can make 8 consecutive integer values starting from 0.
```

**Example 3:**

```
Input: nums = [1,4,10,3,1]
Output: 20
```

**Constraints:**

-   `coins.length == n`
-   `1 <= n <= 4 * $10^4$`
-   `1 <= coins[i] <= 4 * $10^4$`

```cpp
from typing import List

class Solution:
    def getMaximumConsecutive(self, coins: List[int]) -> int:
        s = 1
        coins.sort()
        for x in coins:
            if s >= x:
                s += x
            else:
                break
        return s
```

## [LeetCode 1851. Minimum Interval to Include Each Query](https://leetcode.cn/problems/minimum-interval-to-include-each-query/description/)

You are given a 2D integer array `intervals`, where `intervals[i] = [$left_i$, $right_i$]` describes the$`i^{th}`$ interval starting at$`left_i`$ and ending at$`right_i`$ **(inclusive)**. The **size** of an interval is defined as the number of integers it contains, or more formally$`right_i$ - $left_i$ + 1`.

You are also given an integer array `queries`. The answer to the$`j^{th}`$ query is the **size of the smallest interval** `i` such that$`left_i$ <= queries[j] <= $right_i$`. If no such interval exists, the answer is `-1`.

Return _an array containing the answers to the queries_.

**Example 1:**

```
Input: intervals = [[1,4],[2,4],[3,6],[4,4]], queries = [2,3,4,5]
Output: [3,3,1,4]
Explanation: The queries are processed as follows:
- Query = 2: The interval [2,4] is the smallest interval containing 2. The answer is 4 - 2 + 1 = 3.
- Query = 3: The interval [2,4] is the smallest interval containing 3. The answer is 4 - 2 + 1 = 3.
- Query = 4: The interval [4,4] is the smallest interval containing 4. The answer is 4 - 4 + 1 = 1.
- Query = 5: The interval [3,6] is the smallest interval containing 5. The answer is 6 - 3 + 1 = 4.
```

**Example 2:**

```
Input: intervals = [[2,3],[2,5],[1,8],[20,25]], queries = [2,19,5,22]
Output: [2,-1,4,6]
Explanation: The queries are processed as follows:
- Query = 2: The interval [2,3] is the smallest interval containing 2. The answer is 3 - 2 + 1 = 2.
- Query = 19: None of the intervals contain 19. The answer is -1.
- Query = 5: The interval [2,5] is the smallest interval containing 5. The answer is 5 - 2 + 1 = 4.
- Query = 22: The interval [20,25] is the smallest interval containing 22. The answer is 25 - 20 + 1 = 6.
```

**Constraints:**

-   `1 <= intervals.length <= $10^5$`
-   `1 <= queries.length <= $10^5$`
-   `intervals[i].length == 2`
-   `1 <= lefti <= righti <= $10^7$`
-   `1 <= queries[j] <= $10^7$`

### Union-Find

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
    vector<int> points, p, w;

    int find(int x) {
        if (x != p[x]) p[x] = find(p[x]);
        return p[x];
    }

    int get(int x) {
        return lower_bound(points.begin(), points.end(), x) - points.begin();
    }

public:
    vector<int> minInterval(vector<vector<int>> &intervals, vector<int> &queries) {
        for (auto &interval: intervals) points.push_back(interval[0]), points.push_back(interval[1]);
        for (int q: queries) points.push_back(q);
        sort(points.begin(), points.end());
        points.erase(unique(points.begin(), points.end()), points.end());
        int n = (int) points.size();
        p.resize(n + 1), w.resize(n + 1, -1);
        for (int i = 0; i < n + 1; i++) p[i] = i;
        sort(intervals.begin(), intervals.end(), [](vector<int> &a, vector<int> &b) {
            return a[1] - a[0] < b[1] - b[0];
        });
        for (auto &interval: intervals) {
            int l = get(interval[0]), r = get(interval[1]), len = interval[1] - interval[0] + 1;
            while (find(l) <= r) {
                l = find(l);
                w[l] = len;
                p[l] = l + 1;
            }
        }
        vector<int> res;
        res.reserve(queries.size());
        for (int q: queries) res.push_back(w[get(q)]);
        return res;
    }
};
```

### multiset

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
    struct Event {
        int type, pos, param;

        bool operator<(const Event &that) const {
            return pos < that.pos || (pos == that.pos && type < that.type);
        }
    };

public:
    vector<int> minInterval(vector<vector<int>> &intervals, vector<int> &queries) {
        int m = (int) queries.size();
        vector<Event> events;
        events.reserve(m);
        for (int i = 0; i < m; i++) {
            events.push_back({1, queries[i], i});
        }
        for (const auto &interval: intervals) {
            events.push_back({0, interval[0], interval[1]});
            events.push_back({2, interval[1], interval[0]});
        }

        sort(events.begin(), events.end());

        vector<int> ans(m, -1);
        multiset<int> seg;
        for (const auto &event: events) {
            if (event.type == 0) {
                seg.insert(event.param - event.pos + 1);
            } else if (event.type == 1) {
                if (!seg.empty()) {
                    ans[event.param] = *seg.begin();
                }
            } else {
                int len = event.pos - event.param + 1;
                auto it = seg.find(len);
                seg.erase(it);
            }
        }
        return ans;
    }
};
```

## [LeetCode **2542. Maximum Subsequence Score**](https://leetcode.cn/problems/maximum-subsequence-score/description/)

You are given two **0-indexed** integer arrays `nums1` and `nums2` of equal length `n` and a positive integer `k`. You must choose a **subsequence** of indices from `nums1` of length `k`.

For chosen indices$`i_0`$,$`i_1`$, ...,$`i_k$ - 1`, your **score** is defined as:

-   The sum of the selected elements from `nums1` multiplied with the **minimum** of the selected elements from `nums2`.
-   It can defined simply as: `(nums1[$i_0$] + nums1[$i_1$] +...+ nums1[$i_k$ - 1]) * min(nums2[i0] , nums2[i1], ... ,nums2[$i_k$ - 1])`.

Return _the **maximum** possible score._

A **subsequence** of indices of an array is a set that can be derived from the set `{0, 1, ..., n-1}` by deleting some or no elements.

**Example 1:**

```
Input: nums1 = [1,3,3,2], nums2 = [2,1,3,4], k = 3
Output: 12
Explanation:
The four possible subsequence scores are:
- We choose the indices 0, 1, and 2 with score = (1+3+3) * min(2,1,3) = 7.
- We choose the indices 0, 1, and 3 with score = (1+3+2) * min(2,1,4) = 6.
- We choose the indices 0, 2, and 3 with score = (1+3+2) * min(2,3,4) = 12.
- We choose the indices 1, 2, and 3 with score = (3+3+2) * min(1,3,4) = 8.
Therefore, we return the max score, which is 12.
```

**Example 2:**

```
Input: nums1 = [4,2,3,1,1], nums2 = [7,5,10,9,6], k = 1
Output: 30
Explanation:
Choosing index 2 is optimal: nums1[2] * nums2[2] = 3 * 10 = 30 is the maximum possible score.
```

**Constraints:**

-   `n == nums1.length == nums2.length`
-   `1 <= n <= $10^5$`
-   `0 <= nums1[i], nums2[j] <= $10^5$`
-   `1 <= k <= n`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    long long maxScore(vector<int> &nums1, vector<int> &nums2, int k) {
        using LL = long long;
        using PII = pair<int, int>;

        int n = (int) nums1.size();
        vector<PII> v(n);
        for (int i = 0; i < n; i++) {
            v[i] = {nums2[i], nums1[i]};
        }
        sort(v.begin(), v.end(), greater<>());
        LL res = 0, sum = 0;
        priority_queue<int, vector<int>, greater<>> heap;
        for (int i = 0; i < n; i++) {
            sum += v[i].second;
            if (i >= k - 1) res = max(res, sum * v[i].first);
            heap.push(v[i].second);
            if (heap.size() == k) {
                sum -= heap.top();
                heap.pop();
            }
        }
        return res;
    }
};
```

## [LeetCode 2561. Rearranging Fruits](https://leetcode.cn/problems/rearranging-fruits/description/)

You have two fruit baskets containing `n` fruits each. You are given two **0-indexed** integer arrays `basket1` and `basket2` representing the cost of fruit in each basket. You want to make both baskets **equal**. To do so, you can use the following operation as many times as you want:

-   Chose two indices `i` and `j`, and swap the `i$^{th}$` fruit of `basket1` with the `j$^{th}$`fruit of `basket2`.
-   The cost of the swap is `min(basket1[i],basket2[j])`.

Two baskets are considered equal if sorting them according to the fruit cost makes them exactly the same baskets.

Return _the minimum cost to make both the baskets equal or_ `-1` _if impossible._

**Example 1:**

```
Input: basket1 = [4,2,2,2], basket2 = [1,4,1,2]
Output: 1
Explanation: Swap index 1 of basket1 with index 0 of basket2, which has cost 1. Now basket1 = [4,1,2,2] and basket2 = [2,4,1,2]. Rearranging both the arrays makes them equal.
```

**Example 2:**

```
Input: basket1 = [2,3,4,1], basket2 = [3,2,5,1]
Output: -1
Explanation: It can be shown that it is impossible to make both the baskets equal.
```

**Constraints:**

-   `basket1.length == bakste2.length`
-   `1 <= basket1.length <= $10^5$`
-   `1 <= basket1[i],basket2[i] <= $10^9$`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    long long minCost(vector<int> &basket1, vector<int> &basket2) {
        unordered_map<int, int> cnt;
        for (int i = 0; i < basket1.size(); i++) {
            cnt[basket1[i]]++;
            cnt[basket2[i]]--;
        }
        int mn = INT32_MAX;
        vector<int> v;
        for (auto &[k, c]: cnt) {
            if (c & 1) return -1;
            mn = min(mn, k);
            c = abs(c) / 2;
            while (c--) v.push_back(k);
        }
        nth_element(v.begin(), v.begin() + (int) v.size() / 2, v.end());
        long long res = 0;
        for (int i = 0; i < v.size() / 2; i++) {
            res += min(mn * 2, v[i]);
        }
        return res;
    }
};
```

## [LeetCode **2599. Make the Prefix Sum Non-negative**](https://leetcode.cn/problems/make-the-prefix-sum-non-negative/)
## [LeetCode 2645. Minimum Additions to Make Valid String](https://leetcode.cn/problems/minimum-additions-to-make-valid-string/)
---
Given a string `word` to which you can insert letters "a", "b" or "c" anywhere and any number of times, return *the minimum number of letters that must be inserted so that `word` becomes **valid**.*

A string is called **valid** if it can be formed by concatenating the string "abc" several times.

**Example 1:**

**Input:** word = "b"
**Output:** 2
**Explanation:** Insert the letter "a" right before "b", and the letter "c" right next to "a" to obtain the valid string "**a**b**c**".

**Example 2:**

**Input:** word = "aaa"
**Output:** 6
**Explanation:** Insert letters "b" and "c" next to each "a" to obtain the valid string "a**bc**a**bc**a**bc**".

**Example 3:**

**Input:** word = "abc"
**Output:** 0
**Explanation:** word is already valid. No modifications are needed. 

**Constraints:**

-   `1 <= word.length <= 50`
-   `word` consists of letters "a", "b" and "c" only.
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int addMinimum(string word) {  
        int res = 1, n = (int) word.size();  
        for (int i = 1; i < n; i++) {  
            if (word[i] <= word[i - 1]) res++;  
        }  
        return res * 3 - n;  
    }  
};
```
## [Codeforces 1201 C. Maximum Median](https://codeforces.com/contest/1201/problem/C)
You are given an array 𝑎 of 𝑛 integers, where 𝑛 is odd. You can make the following operation with it:

-   Choose one of the elements of the array (for example 𝑎𝑖) and increase it by 1 (that is, replace it with 𝑎𝑖+11).

You want to make the median of the array the largest possible using at most 𝑘 operations.

The median of the odd-sized array is the middle element after the array is sorted in non-decreasing order. For example, the median of the array [1,5,2,3,5] is 33.

### Input 

The first line contains two integers 𝑛 and 𝑘 (1≤𝑛≤2⋅100000, 𝑛 is odd, 1≤𝑘≤1e9) — the number of elements in the array and the largest number of operations you can make.

The second line contains 𝑛 integers 𝑎1,𝑎2,…,$a_n$ (1≤𝑎𝑖≤1e9).

### Output

Print a single integer — the maximum possible median after the operations.
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
#define rep(i, a, b) for (int (i) = (a); (i) < (b); (i)++)  
  
const int N = 200010;  
  
int n, k;  
int a[N];  
  
void solve() {  
    scanf("%d%d", &n, &k);  
    rep(i, 0, n) scanf("%d", &a[i]);  
    sort(a, a + n);  
    int i = n / 2 + 1;  
    for (; i < n; i++) {  
        if (a[i] != a[i - 1]) {  
            if (a[i] - a[i - 1] > k / (i - n / 2)) break;  
            k -= (i - n / 2) * (a[i] - a[i - 1]);  
        }  
    }  
    int res = a[i - 1] + k / (i - n / 2);  
    printf("%d\n", res);  
}  
  
int main() {  
    solve();  
    return 0;  
}
```
## [Codeforces 1661 C. Water the Trees](https://codeforces.com/contest/1661/problem/C)
There are 𝑛 trees in a park, numbered from 11 to 𝑛. The initial height of the 𝑖-th tree is ℎ𝑖.

You want to water these trees, so they all grow to the same height.

The watering process goes as follows. You start watering trees at day 1. During the 𝑗-th day you can:

-   Choose a tree and water it. If the day is odd (e.g. 1,3,5,7,…), then the height of the tree increases by 1. If the day is even (e.g. 2,4,6,8,…), then the height of the tree increases by 2.
-   Or skip a day without watering any tree.

Note that you can't water more than one tree in a day.

Your task is to determine the minimum number of days required to water the trees so they grow to the same height.

You have to answer 𝑡 independent test cases.

Input

The first line of the input contains one integer 𝑡 (1≤𝑡≤2⋅1e4) — the number of test cases.

The first line of the test case contains one integer 𝑛 (1≤𝑛≤3e5) — the number of trees.

The second line of the test case contains 𝑛 integers ℎ1,ℎ2,…,ℎ𝑛 (1≤ℎ𝑖≤1e9), where ℎ𝑖 is the height of the 𝑖-th tree.

It is guaranteed that the sum of 𝑛 over all test cases does not exceed 3⋅1e5 (∑𝑛≤3⋅105).

Output

For each test case, print one integer — the minimum number of days required to water the trees, so they grow to the same height.

Example

### input

```
3
3
1 2 4
5
4 4 3 5 5
7
2 5 4 8 3 7 4
```


### output
```
4
3
16
```

### Note

Consider the first test case of the example. The initial state of the trees is [1,2,4][1,2,4].

1.  During the first day, let's water the first tree, so the sequence of heights becomes [2,2,4][2,2,4];
2.  during the second day, let's water the second tree, so the sequence of heights becomes [2,4,4][2,4,4];
3.  let's skip the third day;
4.  during the fourth day, let's water the first tree, so the sequence of heights becomes [4,4,4][4,4,4].

Thus, the answer is 4.
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
using LL = long long;  
  
#define rep(i, a, b) for (int (i) = (a); (i) < (b); (i)++)  
  
const int N = 3e5 + 10;  
  
int n;  
int h[N];  
  
void solve() {  
    scanf("%d", &n);  
    rep(i, 0, n) scanf("%d", &h[i]);  
    int mx = *max_element(h, h + n);  
    LL res = INT64_MAX;  
    for (int x: {mx, mx + 1}) {
        LL odd = 0, even = 0;  
        LL days = 0;  
        rep(i, 0, n) {  
            int d = x - h[i];
            even += d / 2;  
            if (d & 1) odd++;  
        }  
        if (odd == even) days = odd * 2;  
        else if (odd < even) {  
            even = (even - odd) * 2;  
            days = odd * 2 + even / 3 * 2 + even % 3;  
        } else {  
            days = odd * 2 - 1;  
        }  
        res = min(res, days);  
    }  
    printf("%lld\n", res);  
}  
  
int main() {  
    int T;  
    scanf("%d", &T);  
    while (T--) solve(); 
    return 0;  
}
```