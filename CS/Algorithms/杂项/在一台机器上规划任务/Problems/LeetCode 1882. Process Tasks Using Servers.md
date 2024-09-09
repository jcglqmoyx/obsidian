---
page-title: LeetCode 1882. Process Tasks Using Servers
url: https://leetcode.cn/problems/process-tasks-using-servers/description/
date: 2024-09-09 14:36:41
---
## Task Assignment Problem

You are given two 0-indexed integer arrays `servers` and `tasks` of lengths `n` and `m` respectively.

- `servers[i]` is the weight of the i-th server.
- `tasks[j]` is the time needed to process the j-th task in seconds.

Tasks are assigned to the servers using a task queue. Initially, all servers are free, and the queue is empty.

### Rules

1. At second `j`, the `j`-th task is inserted into the queue (starting with the 0th task being inserted at second 0).
2. As long as there are free servers and the queue is not empty:
   - The task in the front of the queue will be assigned to a free server with the smallest weight.
   - In case of a tie, it is assigned to a free server with the smallest index.
3. If there are no free servers and the queue is not empty, we wait until a server becomes free and immediately assign the next task.
   - If multiple servers become free at the same time, then multiple tasks from the queue will be assigned in order of insertion following the weight and index priorities above.
4. A server that is assigned task `j` at second `t` will be free again at second `t + tasks[j]`.

### Output

Build an array `ans` of length `m`, where `ans[j]` is the index of the server the `j`-th task will be assigned to.

### Example 1

**Input:**
```plaintext
servers = [3,3,2]
tasks = [1,2,3,2,1,2]
```

**Output:**
```plaintext
[2,2,0,2,1,2]
```

**Explanation:**
Events in chronological order go as follows:
- At second 0, task 0 is added and processed using server 2 until second 1.
- At second 1, server 2 becomes free. Task 1 is added and processed using server 2 until second 3.
- At second 2, task 2 is added and processed using server 0 until second 5.
- At second 3, server 2 becomes free. Task 3 is added and processed using server 2 until second 5.
- At second 4, task 4 is added and processed using server 1 until second 5.
- At second 5, all servers become free. Task 5 is added and processed using server 2 until second 7.

### Example 2

**Input:**
```plaintext
servers = [5,1,4,3,2]
tasks = [2,1,2,4,5,2,1]
```

**Output:**
```plaintext
[1,4,1,4,1,3,2]
```

**Explanation:**
Events in chronological order go as follows:
- At second 0, task 0 is added and processed using server 1 until second 2.
- At second 1, task 1 is added and processed using server 4 until second 2.
- At second 2, servers 1 and 4 become free. Task 2 is added and processed using server 1 until second 4.
- At second 3, task 3 is added and processed using server 4 until second 7.
- At second 4, server 1 becomes free. Task 4 is added and processed using server 1 until second 9.
- At second 5, task 5 is added and processed using server 3 until second 7.
- At second 6, task 6 is added and processed using server 2 until second 7.

### Constraints

- `servers.length == n`
- `tasks.length == m`
- `1 <= n, m <= 2 * 10^5`
- `1 <= servers[i], tasks[j] <= 2 * 10^5`


```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    vector<int> assignTasks(vector<int> &servers, vector<int> &tasks) {
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> available;
        struct Server {
            int next_available_time;
            int weight;
            int idx;

            bool operator<(const Server &t) const {
                if (next_available_time == t.next_available_time) {
                    if (weight == t.weight) {
                        return idx > t.idx;
                    }
                    return weight > t.weight;
                }
                return next_available_time > t.next_available_time;
            }
        };

        priority_queue<Server> occupied;
        for (int i = 0; i < servers.size(); i++) {
            available.emplace(servers[i], i);
        }
        auto n = tasks.size();
        vector<int> res(n);
        for (int time = 0, i = 0; i < n; i++) {
            time = max(time, i);
            while (!occupied.empty() && occupied.top().next_available_time <= time) {
                auto t = occupied.top();
                occupied.pop();
                available.emplace(t.weight, t.idx);
            }
            if (available.empty()) {
                auto t = occupied.top();
                occupied.pop();
                time = t.next_available_time;
                occupied.emplace(time + tasks[i], t.weight, t.idx);
                res[i] = t.idx;
            } else {
                auto t = available.top();
                available.pop();
                occupied.emplace(time + tasks[i], servers[t.second], t.second);
                res[i] = t.second;
            }
        }
        return res;
    }
};
```