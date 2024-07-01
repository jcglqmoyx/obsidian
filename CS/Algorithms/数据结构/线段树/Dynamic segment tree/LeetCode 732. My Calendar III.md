---
page-title: 732. My Calendar III
url: https://leetcode.com/problems/my-calendar-iii/description/
date: 2024-07-01 18:55:28
---
A `k`-booking happens when `k` events have some non-empty intersection (i.e., there is some time that is common to all `k` events.)

You are given some events `[startTime, endTime)`, after each given event, return an integer `k` representing the maximum `k`-booking between all the previous events.

Implement the `MyCalendarThree` class:

- `MyCalendarThree()` Initializes the object.
- `int book(int startTime, int endTime)` Returns an integer `k` representing the largest integer such that there exists a `k`-booking in the calendar.

**Example 1:**

**Input**
["MyCalendarThree", "book", "book", "book", "book", "book", "book"]
[[], [10, 20], [50, 60], [10, 40], [5, 15], [5, 10], [25, 55]]
**Output**
[null, 1, 1, 2, 3, 3, 3]

**Explanation**
MyCalendarThree myCalendarThree = new MyCalendarThree();
myCalendarThree.book(10, 20); // return 1
myCalendarThree.book(50, 60); // return 1
myCalendarThree.book(10, 40); // return 2
myCalendarThree.book(5, 15); // return 3
myCalendarThree.book(5, 10); // return 3
myCalendarThree.book(25, 55); // return 3

**Constraints:**

- `0 <= startTime < endTime <= 109`
- At most `400` calls will be made to `book`.

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 30 * 400 * 4;

int ls[N], rs[N];
int L[N], R[N], idx;
int lazy[N];
int mx[N];

void push_up(int u) {
    mx[u] = max(mx[ls[u]], mx[rs[u]]);
}

void push_down(int u) {
    int mid = (L[u] + R[u]) >> 1;
    if (ls[u] == 0) ls[u] = idx++, L[ls[u]] = L[u], R[ls[u]] = mid;
    if (rs[u] == 0) rs[u] = idx++, L[rs[u]] = mid + 1, R[rs[u]] = R[u];
    if (lazy[u]) {
        mx[ls[u]] += lazy[u], mx[rs[u]] += lazy[u];
        lazy[ls[u]] += lazy[u], lazy[rs[u]] += lazy[u];
        lazy[u] = 0;
    }
}

void update(int u, int l, int r) {
    if (L[u] >= l && R[u] <= r) {
        lazy[u]++;
        mx[u]++;
    } else {
        push_down(u);
        int mid = (L[u] + R[u]) >> 1;
        if (l <= mid) update(ls[u], l, r);
        if (r > mid) update(rs[u], l, r);
        push_up(u);
    }
}

int query(int u, int l, int r) {
    if (L[u] >= l && R[u] <= r) return mx[u];
    push_down(u);
    int mid = (L[u] + R[u]) >> 1;
    int res = 0;
    if (l <= mid) res = query(ls[u], l, r);
    if (r > mid) res = max(res, query(rs[u], l, r));
    push_up(u);
    return res;
}

class MyCalendarThree {
public:
    MyCalendarThree() {
        memset(mx, 0, sizeof mx);
        memset(lazy, 0, sizeof lazy);
        memset(ls, 0, sizeof ls);
        memset(rs, 0, sizeof rs);
        L[0] = 0, R[0] = 1e9;
        idx = 1;
    }

    int book(int startTime, int endTime) {
        update(0, startTime, endTime - 1);
        return query(0, 0, 1e9);
    }
};
```