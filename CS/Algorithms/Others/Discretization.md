# Discretization

## [AcWing 802](https://www.acwing.com/problem/content/804/)

假定有一个无限长的数轴，数轴上每个坐标上的数都是 0。

现在，我们首先进行 $n$ 次操作，每次操作将某一位置 $x$ 上的数加 $c$。

接下来，进行 $m$ 次询问，每个询问包含两个整数 $l$ 和 $r$，你需要求出在区间 $[l, r]$ 之间的所有数的和。

### **输入格式**

第一行包含两个整数 $n$ 和 $m$。

接下来 $n$ 行，每行包含两个整数 $x$ 和 $c$。

再接下来 $m$ 行，每行包含两个整数 $l$ 和 $r$。

### **输出格式**

共 $m$ 行，每行输出一个询问中所求的区间内数字和。

### 数据范围

$−10^9≤x≤10^9$,

$1≤n,m≤10^5$,

$−10^9≤l≤r≤10^9$,

$−10000≤c≤10000$

### **输入样例：**

```
3 3
1 2
3 6
7 5
1 3
4 6
7 8
```

### **输出样例：**

```
8
0
5
```

```cpp
#include <bits/stdc++.h>

using namespace std;

using PII = pair<int, int>;
using VI = vector<int>;

const int N = 300010;

int a[N], s[N];

VI all;
vector<PII> add, query;

int find(int x) {
    int l = 0, r = (int) all.size() - 1;
    while (l < r) {
        int mid = (l + r) >> 1;
        if (all[mid] >= x) r = mid;
        else l = mid + 1;
    }
    return l + 1;
}

int main() {
    int n, m;
    cin >> n >> m;
    vector<PII> op;

    while (n--) {
        int x, c;
        cin >> x >> c;
        add.emplace_back(x, c);
        all.emplace_back(x);
    }

    while (m--) {
        int l, r;
        cin >> l >> r;
        query.push_back({l, r});
        all.push_back(l);
        all.push_back(r);
    }

    sort(all.begin(), all.end());
    all.erase(unique(all.begin(), all.end()), all.end());

    for (auto &item: add) {
        int x = find(item.first);
        a[x] += item.second;
    }

    for (int i = 1; i <= all.size(); i++) {
        s[i] = s[i - 1] + a[i];
    }

    for (auto &item: query) {
        cout << s[find(item.second)] - s[find(item.first) - 1] << endl;
    }

    return 0;
}
```