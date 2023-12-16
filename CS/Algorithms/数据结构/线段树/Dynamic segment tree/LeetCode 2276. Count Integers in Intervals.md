---
page-title: "2276. 统计区间中的整数数目 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/count-integers-in-intervals/description/?envType=daily-question&envId=2023-12-16
date: "2023-12-16 13:25:25"
---
Given an **empty** set of intervals, implement a data structure that can:

-   **Add** an interval to the set of intervals.
-   **Count** the number of integers that are present in **at least one** interval.

Implement the `CountIntervals` class:

-   `CountIntervals()` Initializes the object with an empty set of intervals.
-   `void add(int left, int right)` Adds the interval `[left, right]` to the set of intervals.
-   `int count()` Returns the number of integers that are present in **at least one** interval.

**Note** that an interval `[left, right]` denotes all the integers `x` where `left <= x <= right`.

**Example 1:**

**Input**
\["CountIntervals", "add", "add", "count", "add", "count"\]
\[\[\], \[2, 3\], \[7, 10\], \[\], \[5, 8\], \[\]\]
**Output**
\[null, null, null, 6, null, 8\]

**Explanation**
CountIntervals countIntervals = new CountIntervals(); // initialize the object with an empty set of intervals. 
countIntervals.add(2, 3);  // add \[2, 3\] to the set of intervals.
countIntervals.add(7, 10); // add \[7, 10\] to the set of intervals.
countIntervals.count();    // return 6
                           // the integers 2 and 3 are present in the interval \[2, 3\].
                           // the integers 7, 8, 9, and 10 are present in the interval \[7, 10\].
countIntervals.add(5, 8);  // add \[5, 8\] to the set of intervals.
countIntervals.count();    // return 8
                           // the integers 2 and 3 are present in the interval \[2, 3\].
                           // the integers 5 and 6 are present in the interval \[5, 8\].
                           // the integers 7 and 8 are present in the intervals \[5, 8\] and \[7, 10\].
                           // the integers 9 and 10 are present in the interval \[7, 10\].

**Constraints:**

-   `1 <= left <= right <= 109`
-   At most `105` calls **in total** will be made to `add` and `count`.
-   At least **one** call will be made to `count`.

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class CountIntervals {  
    static constexpr int N = 1700000;  
    int l, r;  
    int res;  
    int idx;  
    int L[N]{}, R[N]{};  
    int ls[N]{}, rs[N]{};  
    bool lazy[N]{};  
    int sum[N]{};  
  
    void push_up(int u) {  
        sum[u] = sum[ls[u]] + sum[rs[u]];  
    }  
  
    void push_down(int u) {  
        if (!ls[u]) ls[u] = idx++;  
        if (!rs[u]) rs[u] = idx++;  
        int mid = L[u] + R[u] >> 1;  
        L[ls[u]] = L[u], R[ls[u]] = mid;  
        L[rs[u]] = mid + 1, R[rs[u]] = R[u];  
        if (lazy[u]) {  
            sum[ls[u]] = R[ls[u]] - L[ls[u]] + 1, sum[rs[u]] = R[rs[u]] - L[rs[u]] + 1;  
            lazy[ls[u]] = lazy[rs[u]] = true;  
            lazy[u] = false;  
        }  
    }  
  
    void update(int u, int l, int r) {  
        if (L[u] >= l && R[u] <= r) {  
            sum[u] = R[u] - L[u] + 1;  
            lazy[u] = true;  
        } else {  
            push_down(u);  
            int mid = L[u] + R[u] >> 1;  
            if (l <= mid) update(ls[u], l, r);  
            if (r > mid) update(rs[u], l, r);  
            push_up(u);  
        }  
    }  
  
public:  
    CountIntervals() {  
        res = 0;  
        idx = 0;  
        L[idx] = 1;  
        R[idx] = 1e9;  
        idx++;  
    }  
  
    void add(int left, int right) {  
        update(0, left, right);  
        res = sum[0];  
    }  
  
    int count() {  
        return res;  
    }  
};
```