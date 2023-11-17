## [LeetCode 1157. Online Majority Element In Subarray](https://leetcode.cn/problems/online-majority-element-in-subarray/)
---
Design a data structure that efficiently finds the **majority element** of a given subarray.

The **majority element** of a subarray is an element that occurs `threshold` times or more in the subarray.

Implementing the `MajorityChecker` class:

-   `MajorityChecker(int[] arr)` Initializes the instance of the class with the given array `arr`.
-   `int query(int left, int right, int threshold)` returns the element in the subarray `arr[left...right]` that occurs at least `threshold` times, or `-1` if no such element exists.

**Example 1:**

**Input**
\["MajorityChecker", "query", "query", "query"\]
\[\[\[1, 1, 2, 2, 1, 1\]\], \[0, 5, 4\], \[0, 3, 3\], \[2, 3, 2\]\]
**Output**
\[null, 1, -1, 2\]

**Explanation**
MajorityChecker majorityChecker = new MajorityChecker(\[1, 1, 2, 2, 1, 1\]);
majorityChecker.query(0, 5, 4); // return 1
majorityChecker.query(0, 3, 3); // return -1
majorityChecker.query(2, 3, 2); // return 2

**Constraints:**

-   `1 <= arr.length <= 2 * 104`
-   `1 <= arr[i] <= 2 * 104`
-   `0 <= left <= right < arr.length`
-   `threshold <= right - left + 1`
-   `2 * threshold > right - left + 1`
-   At most `104` calls will be made to `query`.
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
static constexpr int N = 2e4 + 5;  
  
struct Node {  
    int l, r;  
    int val, cnt;  
  
    Node operator+(const Node &node) const {  
        Node t{min(l, node.l), max(r, node.r)};  
        if (val == node.val) {  
            t.val = val;  
            t.cnt = cnt + node.cnt;  
        } else if (cnt > node.cnt) {  
            t.val = val;  
            t.cnt = cnt - node.cnt;  
        } else {  
            t.val = node.val;  
            t.cnt = node.cnt - cnt;  
        }  
        return t;  
    }  
} tr[N << 2];  
  
class MajorityChecker {  
    vector<int> a;  
    vector<vector<int>> s;  
  
    void build(int u, int l, int r) {  
        tr[u] = {l, r};  
        if (l == r) {  
            tr[u].val = a[l];  
            tr[u].cnt = 1;  
        } else {  
            int mid = (l + r) >> 1;  
            build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);  
            tr[u] = tr[u << 1] + tr[u << 1 | 1];  
        }  
    }  
  
    Node ask(int u, int l, int r) {  
        if (tr[u].l >= l && tr[u].r <= r) return tr[u];  
        int mid = (tr[u].l + tr[u].r) >> 1;  
        if (l > mid) return ask(u << 1 | 1, l, r);  
        if (r <= mid) return ask(u << 1, l, r);  
        return ask(u << 1, l, r) + ask(u << 1 | 1, l, r);  
    }  
  
public:  
    MajorityChecker(vector<int> &arr) {  
        a.resize(N);  
        s.resize(N);  
        for (int i = 0; i < arr.size(); i++) {  
            a[i] = arr[i];  
            s[arr[i]].push_back(i);  
        }  
        build(1, 0, N - 1);  
    }  
  
    int query(int left, int right, int threshold) {  
        auto t = ask(1, left, right);  
        auto l = lower_bound(s[t.val].begin(), s[t.val].end(), left);  
        auto r = upper_bound(s[t.val].begin(), s[t.val].end(), right);  
        if (r - l >= threshold) return t.val;  
        return -1;  
    }  
};
```