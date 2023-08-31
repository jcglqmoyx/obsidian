[LeetCode 239](https://leetcode.cn/problems/sliding-window-maximum/)

## [AcWing 154](https://www.acwing.com/problem/content/156/)

给定一个大小为 $n$ ≤ $10^6$ 的数组。

有一个大小为 $k$ 的滑动窗口，它从数组的最左边移动到最右边。

你只能在窗口中看到 $k$ 个数字。

每次滑动窗口向右移动一个位置。

以下是一个例子：

该数组为 `[1 3 -1 -3 5 3 6 7]`，$k$ 为 $3$。

|       窗口位置 |        最小值 |         最大值 |
| --- | --- | --- |
| [1 3 -1] -3 5 3 6 7 |          -1 |             3 |
| 1 [3 -1 -3] 5 3 6 7 |          -3 |             3 |
| 1 3 [-1 -3 5] 3 6 7 |          -3 |             5  |
| 1 3 -1 [-3 5 3] 6 7 |          -3 |             5 |
| 1 3 -1 -3 [5 3 6] 7 |           3 |             6 |
| 1 3 -1 -3 5 [3 6 7] |           3 |             7 |

你的任务是确定滑动窗口位于每个位置时，窗口中的最大值和最小值。

### **输入格式**

输入包含两行。

第一行包含两个整数 $n$ 和 $k$，分别代表数组长度和滑动窗口的长度。

第二行有 $n$ 个整数，代表数组的具体数值。

同行数据之间用空格隔开。

### **输出格式**

输出包含两个。

第一行输出，从左至右，每个位置滑动窗口中的最小值。

第二行输出，从左至右，每个位置滑动窗口中的最大值。

### **输入样例：**

```
8 3
1 3 -1 -3 5 3 6 7
```

### **输出样例：**

```diff
-1 -3 -3 -3 3 3
3 3 5 5 6 7
```

### Using STL:

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1000010;

int n, k;
int a[N];

int main() {
    scanf("%d%d", &n, &k);
    for (int i = 0; i < n; i++) {
        scanf("%d", &a[i]);
    }
    deque<int> dq;
    for (int i = 0; i < n; i++) {
        if (!dq.empty() && i - k + 1 > dq.front()) dq.pop_front();
        while (!dq.empty() && a[dq.back()] >= a[i]) dq.pop_back();
        dq.push_back(i);
        if (i >= k - 1) printf("%d ", a[dq.front()]);
    }
    puts("");
    dq.clear();
    for (int i = 0; i < n; i++) {
        if (!dq.empty() && i - k + 1 > dq.front()) dq.pop_front();
        while (!dq.empty() && a[dq.back()] <= a[i]) dq.pop_back();
        dq.push_back(i);
        if (i >= k - 1) printf("%d ", a[dq.front()]);
    }
    puts("");
    return 0;
}
```

### Not using STL:

```
#include <bits/stdc++.h>

using namespace std;

const int N = 1000010;

int n, k;
int a[N];
int q[N], hh, tt;

int main() {
    scanf("%d%d", &n, &k);
    for (int i = 0; i < n; i++) {
        scanf("%d", &a[i]);
    }
    hh = 0, tt = 1;
    for (int i = 0; i < n; i++) {
        if (hh <= tt && i - k + 1 > q[hh]) hh++;
        while (hh <= tt & a[q[tt]] >= a[i]) tt--;
        q[++tt] = i;
        if (i >= k - 1) printf("%d ", a[q[hh]]);
    }
    puts("");
    hh = 0, tt = -1;
    for (int i = 0; i < n; i++) {
        if (hh <= tt && i - k + 1 > q[hh]) hh++;
        while (hh <= tt & a[q[tt]] <= a[i]) tt--;
        q[++tt] = i;
        if (i >= k - 1) printf("%d ", a[q[hh]]);
    }
    puts("");
    return 0;
}
```

## [LeetCode **918. Maximum Sum Circular Subarray**](https://leetcode.cn/problems/maximum-sum-circular-subarray/)

Given a **circular integer array** `nums` of length `n`, return *the maximum possible sum of a non-empty **subarray** of* `nums`.

A **circular array** means the end of the array connects to the beginning of the array. Formally, the next element of `nums[i]` is `nums[(i + 1) % n]` and the previous element of `nums[i]` is `nums[(i - 1 + n) % n]`.

A **subarray** may only include each element of the fixed buffer `nums` at most once. Formally, for a subarray `nums[i], nums[i + 1], ..., nums[j]`, there does not exist `i <= $k_1$`, $k_2$ `<= j` with $`k_1$ % n == $k_2$ % n`.

**Example 1:**

```
**Input**: nums = [1,-2,3,-2]
**Output**: 3
**Explanation**: Subarray [3] has maximum sum 3.
```

**Example 2:**

```
**Input**: nums = [5,-3,5]
**Output**: 10
**Explanation**: Subarray [5,5] has maximum sum 5 + 5 = 10.
```

**Example 3:**

```
**Input**: nums = [-3,-2,-3]
**Output**: -2
**Explanation**: Subarray [-2] has maximum sum -2.
```

**Constraints:**

- `n == nums.length`
- `1 <= n <= 3 * $10^4$`
- `3 * $10^4$ <= nums[i] <= 3 * $10^4$`

### C++

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int maxSubarraySumCircular(vector<int> &nums) {
        int n = (int) nums.size(), res = INT32_MIN;
        int s[n * 2];
        memset(s, 0, sizeof s);
        for (int i = 1; i < n * 2; i++) {
            s[i] = s[i - 1] + nums[(i - 1) % n];
        }
        deque<int> dq = {0};
        for (int i = 1; i < n * 2; i++) {
            while (!dq.empty() && i - dq.front() > n) dq.pop_front();
            if (!dq.empty()) res = max(res, s[i] - s[dq.front()]);
            while (!dq.empty() && s[dq.back()] >= s[i]) dq.pop_back();
            dq.push_back(i);
        }
        return res;
    }
};
```

### Python3

```cpp
from typing import List

class Solution:
    def maxSubarraySumCircular(self, nums: List[int]) -> int:
        n, res = len(nums), float('-inf')
        s = [0] * (n * 2)
        for i in range(1, n * 2):
            s[i] = s[i - 1] + nums[(i - 1) % n]
        q = [0]
        for i in range(1, n * 2):
            while q and i - q[0] > n:
                q = q[1:]
            if q:
                res = max(res, s[i] - s[q[0]])
            while q and s[q[-1]] >= s[i]:
                q = q[:-1]
            q.append(i)
        return res
```