## [AcWing 803. 区间合并](https://www.acwing.com/problem/content/description/805/)

给定 $n$ 个区间$[l, r]$，要求合并所有有交集的区间。

注意如果在端点处相交，也算有交集。

输出合并完成后的区间个数。

例如：[1,3] 和 [2,6] 可以合并为一个区间 [1,6]。

### **输入格式**

第一行包含整数 $n$。

接下来 $n$ 行，每行包含两个整数 $l$ 和 $r$。

### **输出格式**

共一行，包含一个整数，表示合并区间完成后的区间个数。

### **数据范围**

1 ≤ n ≤ 100000,

$-10^9$≤ $l_i$ ≤ $r_i$ ≤ $10^9$

### **输入样例：**

```
5
1 2
2 4
5 6
7 8
7 9
```

### **输出样例：**

```
3
```

```cpp
#include <bits/stdc++.h>

using namespace std;

using PII = pair<int, int>;

int n;

int merge(vector<PII> &v) {
    sort(v.begin(), v.end());
    int res = 1, end = v[0].second;
    for (int i = 1; i < n; i++) {
        if (v[i].first <= end) end = max(end, v[i].second);
        else res++, end = v[i].second;
    }
    return res;
}

int main() {
    scanf("%d", &n);
    vector<PII> v(n);
    for (int i = 0; i < n; i++) {
        int l, r;
        scanf("%d%d", &l, &r);
        v[i] = {l, r};
    }
    int t = merge(v);
    printf("%d\n", t);
    return 0;
}
```
## [AcWing **905. 区间选点**](https://www.acwing.com/problem/content/907/)

给定 N 个闭区间 $[a_i, b_i]$，请你在数轴上选择尽量少的点，使得每个区间内至少包含一个选出的点。

输出选择的点的最小数量。

位于区间端点上的点也算作区间内。

### **输入格式**

第一行包含整数 N，表示区间数。

接下来 N 行，每行包含两个整数 $a_i$，$b_i$，表示一个区间的两个端点。

### **输出格式**

输出一个整数，表示所需的点的最小数量。

### **数据范围**

1 ≤ N ≤ $10^5$,

-$10^9$ ≤ $a_i$，$b_i$ ≤ $10^9$

### **输入样例：**

```diff
3
-1 1
2 4
3 5
```

### **输出样例：**

```
2
```

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
    int n;
    cin >> n;
    vector<vector<int>> segs;
    for (int i = 0; i < n; i++) {
        int x, y;
        cin >> x >> y;
        segs.push_back({x, y});
    }
    sort(segs.begin(), segs.end(), [](vector<int> &a, vector<int> &b) {
        return a[1] < b[1];
    });
    int cnt = 1;
    int end = segs[0][1];
    for (int i = 1; i < n; i++) {
        if (end >= segs[i][0] && end <= segs[i][1]) continue;
        else end = segs[i][1], cnt++;
    }
    cout << cnt << endl;
    return 0;
}
```

## [AcWing **906. 区间分组**](https://www.acwing.com/problem/content/908/)

给定 N 个闭区间 $[a_i,\ b_i]$，请你将这些区间分成若干组，使得每组内部的区间两两之间（包括端点）没有交集，并使得组数尽可能小。

输出最小组数。

### **输入格式**

第一行包含整数 N，表示区间数。

接下来 N 行，每行包含两个整数 $a_i$，$b_i$，表示一个区间的两个端点。

### **输出格式**

输出一个整数，表示最小组数。

### **数据范围**

1 ≤ N ≤ $10^5$,

-$10^9$ ≤ $a_i$，$b_i$ ≤ $10^9$

### **输入样例：**

```diff
3
-1 1
2 4
3 5
```

### **输出样例：**

```
2
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 100010;

int n;

struct Range {
    int l, r;

    bool operator<(const Range &range) const {
        return l < range.l;
    }
} ranges[N];

int solve() {
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%d%d", &ranges[i].l, &ranges[i].r);
    }
    sort(ranges, ranges + n);
    priority_queue<int, vector<int>, greater<>> heap;
    for (int i = 0; i < n; i++) {
        if (heap.empty()) {
            heap.push(ranges[i].r);
        } else {
            if (ranges[i].l > heap.top()) {
                heap.pop();
                heap.push(ranges[i].r);
            } else {
                heap.push(ranges[i].r);
            }
        }
    }
    return (int) heap.size();
}

int main() {
    printf("%d", solve());
    return 0;
}
```

[优化](https://www.acwing.com/solution/content/8902/)

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 100100;

int n;
int b[2 * N], idx;

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        int l, r;
        scanf("%d %d", &l, &r);
        b[idx++] = l * 2; // 标记左端点为偶数。
        b[idx++] = r * 2 + 1; // 标记右端点为奇数。
    }

    sort(b, b + idx);

    int res = 1, t = 0;
    for (int i = 0; i < idx; i++) {
        if (b[i] % 2 == 0) t++;
        else t--;
        res = max(res, t);
    }
    printf("%d\n", res);
    return 0;
}
```

## [AcWing **907. 区间覆盖**](https://www.acwing.com/problem/content/description/909/)

给定 N 个闭区间 $[a_i, b_i]$ 以及一个线段区间 $[s, t]$，请你选择尽量少的区间，将指定线段区间完全覆盖。

输出最少区间数，如果无法完全覆盖则输出 −1。

### **输入格式**

第一行包含两个整数 s 和 t，表示给定线段区间的两个端点。

第二行包含整数 N，表示给定区间数。

接下来 N 行，每行包含两个整数 $a_i, b_i$，表示一个区间的两个端点。

### **输出格式**

输出一个整数，表示所需最少区间数。

如果无解，则输出 −1。

### **数据范围**

1 ≤ N ≤ $10^5$，

-$10^9$ ≤ $a_i$ ≤ $b_i$ ≤ $10^9$，

-$10^9$ ≤ $s$ ≤ $t$ ≤ $10^9$，

### **输入样例：**

```diff
1 5
3
-1 3
2 4
3 5
```

### **输出样例：**

```
2
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 100010;

int n;

struct Range {
    int l, r;

    bool operator<(const Range &W) const {
        return l < W.l;
    }
} range[N];

int main() {
    int st, ed;
    scanf("%d%d", &st, &ed);
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        int l, r;
        scanf("%d%d", &l, &r);
        range[i] = {l, r};
    }

    sort(range, range + n);

    int res = 0;
    bool success = false;
    for (int i = 0; i < n; i++) {
        int j = i, r = -2e9;
        while (j < n && range[j].l <= st) {
            r = max(r, range[j].r);
            j++;
        }

        if (r < st) {
            res = -1;
            break;
        }

        res++;
        if (r >= ed) {
            success = true;
            break;
        }

        st = r;
        i = j - 1;
    }

    if (!success) res = -1;
    printf("%d\n", res);

    return 0;
}
```

## [AcWing **908. 最大不相交区间数量**](https://www.acwing.com/problem/content/910/)

给定 N 个闭区间 $[a_i, b_i]$，请你在数轴上选择若干区间，使得选中的区间之间互不相交（包括端点）。

输出可选取区间的最大数量。

### **输入格式**

第一行包含整数 N，表示区间数。

接下来 N 行，每行包含两个整数 $a_i$，$b_i$，表示一个区间的两个端点。

### **输出格式**

输出一个整数，表示可选取区间的最大数量。

### **数据范围**

1 ≤ N ≤ $10^5$，

-$10^9$ ≤ $a_i$，$b_i$ ≤ $10^9$

### **输入样例：**

```diff
3
-1 1
2 4
3 5
```

### **输出样例：**

```
2
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 100010;

int n;

struct Range {
    int l, r;

    bool operator<(const Range &W) const {
        return r < W.r;
    }
} range[N];

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%d%d", &range[i].l, &range[i].r);
    }

    sort(range, range + n);

    int res = 0, ed = -2e9;
    for (int i = 0; i < n; i++)
        if (ed < range[i].l) {
            res++;
            ed = range[i].r;
        }

    printf("%d\n", res);

    return 0;
}
```

## [LeetCode 57 Insert Interval](https://leetcode.cn/problems/insert-interval/description/)

You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [$start_i$, end$_i$]` represent the start and the end of the `i$^{th}$` interval and `intervals` is sorted in ascending order by `start$_i$`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` *after the insertion*.

**Example 1:**

```
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

**Example 2:**

```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
```

**Constraints:**

- `0 <= intervals.length <= $10^4$`
- `intervals[i].length == 2`
- `0 <= start$_i$ <= end$_i$ <= 105`
- `intervals` is sorted by `start$_i$` in **ascending** order.
- `newInterval.length == 2`
- `0 <= start <= end <= $10^5$`

```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>> &intervals, vector<int> &newInterval) {
        vector<vector<int>> res;
        bool used = false;
        int left = newInterval[0], right = newInterval[1];
        for (vector<int> &interval : intervals) {
            if (interval[0] > right) {
                if (!used) {
                    res.push_back({left, right});
                    used = true;
                }
                res.push_back(interval);
            } else if (interval[1] < left) res.push_back(interval);
            else {
                left = min(left, interval[0]);
                right = max(right, interval[1]);
            }
        }
        if (!used) res.push_back({left, right});
        return res;
    }
};
```
