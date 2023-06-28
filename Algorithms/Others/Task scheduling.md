## [LeetCode **1834. Single-Threaded CPU**](https://leetcode.cn/problems/single-threaded-cpu/description/)

You are given `n` tasks labeled from `0` to `n - 1` represented by a 2D integer array `tasks`, where `tasks[i] = [$enqueueTime_i$, $processingTime_i$]`means that the `ith` task will be available to process at$`enqueueTime_i`$and will take$`processingTime_i`$to finish processing.

You have a single-threaded CPU that can process **at most one** task at a time and will act in the following way:

- If the CPU is idle and there are no available tasks to process, the CPU remains idle.
- If the CPU is idle and there are available tasks, the CPU will choose the one with the **shortest processing time**. If multiple tasks have the same shortest processing time, it will choose the task with the smallest index.
- Once a task is started, the CPU will **process the entire task** without stopping.
- The CPU can finish a task then start a new one instantly.

Return *the order in which the CPU will process the tasks.*

**Example 1:**

```
Input: tasks = [[1,2],[2,4],[3,2],[4,1]]
Output: [0,2,3,1]
Explanation:The events go as follows:
- At time = 1, task 0 is available to process. Available tasks = {0}.
- Also at time = 1, the idle CPU starts processing task 0. Available tasks = {}.
- At time = 2, task 1 is available to process. Available tasks = {1}.
- At time = 3, task 2 is available to process. Available tasks = {1, 2}.
- Also at time = 3, the CPU finishes task 0 and starts processing task 2 as it is the shortest. Available tasks = {1}.
- At time = 4, task 3 is available to process. Available tasks = {1, 3}.
- At time = 5, the CPU finishes task 2 and starts processing task 3 as it is the shortest. Available tasks = {1}.
- At time = 6, the CPU finishes task 3 and starts processing task 1. Available tasks = {}.
- At time = 10, the CPU finishes task 1 and becomes idle.
```

**Example 2:**

```
Input: tasks = [[7,10],[7,12],[7,5],[7,4],[7,2]]
Output: [4,3,2,0,1]
Explanation:The events go as follows:
- At time = 7, all the tasks become available. Available tasks = {0,1,2,3,4}.
- Also at time = 7, the idle CPU starts processing task 4. Available tasks = {0,1,2,3}.
- At time = 9, the CPU finishes task 4 and starts processing task 3. Available tasks = {0,1,2}.
- At time = 13, the CPU finishes task 3 and starts processing task 2. Available tasks = {0,1}.
- At time = 18, the CPU finishes task 2 and starts processing task 0. Available tasks = {1}.
- At time = 28, the CPU finishes task 0 and starts processing task 1. Available tasks = {}.
- At time = 40, the CPU finishes task 1 and becomes idle.
```

**Constraints:**

- `tasks.length == n`
- `1 <= n <= $10^5$`
- `1 <= $enqueueTime_i$, $processingTime_i$ <= $10^9$`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    vector<int> getOrder(vector<vector<int>> &tasks) {
        using PII = pair<int, int>;
        using LL = long long;
        int n = (int) tasks.size();
        vector<int> indices(n);
        iota(indices.begin(), indices.end(), 0);
        sort(indices.begin(), indices.end(), [&](int i, int j) {
            return tasks[i][0] < tasks[j][0];
        });

        vector<int> ans;
        priority_queue<PII, vector<PII>, greater<>> q;
        LL timestamp = 0;
        int ptr = 0;
        for (int i = 0; i < n; i++) {
            if (q.empty()) {
                timestamp = max(timestamp, (LL) tasks[indices[ptr]][0]);
            }
            while (ptr < n && tasks[indices[ptr]][0] <= timestamp) {
                q.emplace(tasks[indices[ptr]][1], indices[ptr]);
                ptr++;
            }
            auto&&[process, index] = q.top();
            timestamp += process;
            ans.push_back(index);
            q.pop();
        }
        return ans;
    }
};
```

## [LeetCode **2532. Time to Cross a Bridge**](https://leetcode.cn/problems/time-to-cross-a-bridge/)

There are `k` workers who want to move `n` boxes from an old warehouse to a new one. You are given the two integers `n` and `k`, and a 2D integer array `time` of size `k x 4` where `time[i] = [$leftToRight_i$, $pickOld_i$, $rightToLeft_i$, $putNew_i$]`.

The warehouses are separated by a river and connected by a bridge. The old warehouse is on the right bank of the river, and the new warehouse is on the left bank of the river. Initially, all `k` workers are waiting on the left side of the bridge. To move the boxes, the$`i^{th}`$ worker (**0-indexed**) can :

- Cross the bridge from the left bank (new warehouse) to the right bank (old warehouse) in `l$eftToRight_i$` minutes.
- Pick a box from the old warehouse and return to the bridge in$`pickOld_i`$ minutes. Different workers can pick up their boxes simultaneously.
- Cross the bridge from the right bank (old warehouse) to the left bank (new warehouse) in$`rightToLeft_i`$ minutes.
- Put the box in the new warehouse and return to the bridge in `putNewi` minutes. Different workers can put their boxes simultaneously.

A worker `i` is **less efficient** than a worker `j` if either condition is met:

- $`leftToRight_i$ + $rightToLeft_i$ > $leftToRight_j$ + $rightToLeft_j$`
- $`leftToRight_i$ + $rightToLeft_i$ == $leftToRight_j$ + $rightToLeft_j$` and `i > j`

The following rules regulate the movement of the workers through the bridge :

- If a worker `x` reaches the bridge while another worker `y` is crossing the bridge, `x` waits at their side of the bridge.
- If the bridge is free, the worker waiting on the right side of the bridge gets to cross the bridge. If more than one worker is waiting on the right side, the one with **the lowest efficiency** crosses first.
- If the bridge is free and no worker is waiting on the right side, and at least one box remains at the old warehouse, the worker on the left side of the river gets to cross the bridge. If more than one worker is waiting on the left side, the one with **the lowest efficiency** crosses first.

Return *the instance of time at which the last worker **reaches the left bank** of the river after all n boxes have been put in the new warehouse*.

**Example 1:**

```
Input: n = 1, k = 3, time = [[1,1,2,1],[1,1,3,1],[1,1,4,1]]
Output: 6
Explanation:
From 0 to 1: worker 2 crosses the bridge from the left bank to the right bank.
From 1 to 2: worker 2 picks up a box from the old warehouse.
From 2 to 6: worker 2 crosses the bridge from the right bank to the left bank.
From 6 to 7: worker 2 puts a box at the new warehouse.
The whole process ends after 7 minutes. We return 6 because the problem asks for the instance of time at which the last worker reaches the left bank.
```

**Example 2:**

```
Input: n = 3, k = 2, time = [[1,9,1,8],[10,10,10,10]]
Output: 50
Explanation:
From 0  to 10: worker 1 crosses the bridge from the left bank to the right bank.
From 10 to 20: worker 1 picks up a box from the old warehouse.
From 10 to 11: worker 0 crosses the bridge from the left bank to the right bank.
From 11 to 20: worker 0 picks up a box from the old warehouse.
From 20 to 30: worker 1 crosses the bridge from the right bank to the left bank.
From 30 to 40: worker 1 puts a box at the new warehouse.
From 30 to 31: worker 0 crosses the bridge from the right bank to the left bank.
From 31 to 39: worker 0 puts a box at the new warehouse.
From 39 to 40: worker 0 crosses the bridge from the left bank to the right bank.
From 40 to 49: worker 0 picks up a box from the old warehouse.
From 49 to 50: worker 0 crosses the bridge from the right bank to the left bank.
From 50 to 58: worker 0 puts a box at the new warehouse.
The whole process ends after 58 minutes. We return 50 because the problem asks for the instance of time at which the last worker reaches the left bank.
```

**Constraints:**

- `1 <= n, k <= 10000`
- `time.length == k`
- `time[i].length == 4`
- `1 <= $leftToRight_i$, $pickOld_i$, $rightToLeft_i$, $putNew_i$ <= 1000`

```cpp
#include <bits/stdc++.h>

using namespace std;

/*
 * endlesscheng https://leetcode.cn/problems/time-to-cross-a-bridge/solution/by-endlesscheng-nzqo/
 */
class Solution {
public:
    int findCrossingTime(int n, int k, vector<vector<int>> &time) {
        stable_sort(time.begin(), time.end(), [](auto &a, auto &b) {
            return a[0] + a[2] < b[0] + b[2];
        });
        priority_queue<pair<int, int>> waitL, waitR;
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> workL, workR;
        for (int i = k - 1; i >= 0; --i) waitL.emplace(i, 0);
        int cur = 0;
        while (n) {
            while (!workL.empty() && workL.top().first <= cur) {
                auto[t, i] = workL.top();
                workL.pop();
                waitL.emplace(i, t);
            }
            while (!workR.empty() && workR.top().first <= cur) {
                auto[t, i] = workR.top();
                workR.pop();
                waitR.emplace(i, t);
            }
            if (!waitR.empty()) {
                auto[i, t] = waitR.top();
                waitR.pop();
                cur += time[i][2];
                workL.emplace(cur + time[i][3], i);
            } else if (!waitL.empty()) {
                auto[i, t] = waitL.top();
                waitL.pop();
                cur += time[i][0];
                workR.emplace(cur + time[i][1], i);
                --n;
            } else if (workL.empty()) cur = workR.top().first;
            else if (workR.empty()) cur = workL.top().first;
            else cur = min(workL.top().first, workR.top().first);
        }
        while (!workR.empty()) {
            auto[t, i] = workR.top();
            workR.pop();
            cur = max(t, cur) + time[i][2];
        }
        return cur;
    }
};
```