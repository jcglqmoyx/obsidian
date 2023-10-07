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