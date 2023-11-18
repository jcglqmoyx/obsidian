---
page-title: "Range Module - LeetCode"
url: https://leetcode.com/problems/range-module/description/?envType=daily-question&envId=2023-11-12
date: "2023-11-18 09:32:59"
---
A Range Module is a module that tracks ranges of numbers. Design a data structure to track the ranges represented as **half-open intervals** and query about them.

A **half-open interval** `[left, right)` denotes all the real numbers `x` where `left <= x < right`.

Implement the `RangeModule` class:

-   `RangeModule()` Initializes the object of the data structure.
-   `void addRange(int left, int right)` Adds the **half-open interval** `[left, right)`, tracking every real number in that interval. Adding an interval that partially overlaps with currently tracked numbers should add any numbers in the interval `[left, right)` that are not already tracked.
-   `boolean queryRange(int left, int right)` Returns `true` if every real number in the interval `[left, right)` is currently being tracked, and `false` otherwise.
-   `void removeRange(int left, int right)` Stops tracking every real number currently being tracked in the **half-open interval** `[left, right)`.

**Example 1:**

**Input**
\["RangeModule", "addRange", "removeRange", "queryRange", "queryRange", "queryRange"\]
\[\[\], \[10, 20\], \[14, 16\], \[10, 14\], \[13, 15\], \[16, 17\]\]
**Output**
\[null, null, null, true, false, true\]

**Explanation**
RangeModule rangeModule = new RangeModule();
rangeModule.addRange(10, 20);
rangeModule.removeRange(14, 16);
rangeModule.queryRange(10, 14); // return True,(Every number in \[10, 14) is being tracked)
rangeModule.queryRange(13, 15); // return False,(Numbers like 14, 14.03, 14.17 in \[13, 15) are not being tracked)
rangeModule.queryRange(16, 17); // return True, (The number 16 in \[16, 17) is still being tracked, despite the remove operation)

**Constraints:**

-   `1 <= left < right <= 109`
-   At most `104` calls will be made to `addRange`, `queryRange`, and `removeRange`.

```cpp
class RangeModule {
public:
    RangeModule() {
        low[0] = 1, high[0] = 1e9, cnt++;
    }

    void addRange(int left, int right) {
        update(0, left, right - 1, 1);
    }

    bool queryRange(int left, int right) {
        return query(0, left, right - 1);
    }

    void removeRange(int left, int right) {
        update(0, left, right - 1, -1);
    }

private:
    static constexpr int N = 200010, M = N << 2;

    int cnt{};
    int ls[M]{}, rs[M]{}, low[M]{}, high[M]{};
    bool cover[M]{};
    int lazy[M]{};

    void push_up(int u) {
        cover[u] = cover[ls[u]] & cover[rs[u]];
    }

    void push_down(int u) {
        if (!ls[u]) ls[u] = cnt++;
        if (!rs[u]) rs[u] = cnt++;
        int mid = (low[u] + high[u]) >> 1;
        low[ls[u]] = low[u], high[ls[u]] = mid;
        low[rs[u]] = mid + 1, high[rs[u]] = high[u];
        if (lazy[u]) {
            lazy[ls[u]] = lazy[rs[u]] = lazy[u];
            cover[ls[u]] = cover[rs[u]] = (lazy[u] == 1);
            lazy[u] = 0;
        }
    }

    void update(int u, int l, int r, int change) {
        if (low[u] >= l && high[u] <= r) {
            cover[u] = (change == 1);
            lazy[u] = change;
        } else {
            push_down(u);
            int mid = (low[u] + high[u]) >> 1;
            if (l <= mid) update(ls[u], l, r, change);
            if (r > mid) update(rs[u], l, r, change);
            push_up(u);
        }
    }

    bool query(int u, int l, int r) {
        if (low[u] >= l && high[u] <= r) return cover[u];
        push_down(u);
        bool res = true;
        int mid = (low[u] + high[u]) >> 1;
        if (l <= mid) res = query(ls[u], l, r);
        if (r > mid) res &= query(rs[u], l, r);
        return res;
    }
};
```