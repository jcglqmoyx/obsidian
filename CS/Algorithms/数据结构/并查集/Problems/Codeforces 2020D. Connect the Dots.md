# Connect the Dots

## 题面翻译

### 题意描述

爱丽丝画了一条直线，并在上面标记了$n$个点，从1到$n$进行索引。最初，点之间没有弧线，所以它们都是不相交的。之后，Alice执行以下类型的$m$个操作：

1. 她选了三个整数$a_i$、$d_i$ 和 $k_i$。
2. 她选择点$a_i$、$a_i+d_i$、$a_i+2d_i$ …… $a_i+k_i \cdot d_i$，并用弧线连接这些点的每一对。

完成所有$m$个操作后，她想知道这些点形成的连接组件的数量。

如果两个点之间通过若干（**可能为零**）弧线和其他点存在路径，则称这两个点位于一个连接的组件中。

### 输入格式

每个测试包含多个测试用例。第一行包含测试用例的数量$t$。每个测试用例的描述如下：

- 每个测试用例的第一行包含两个整数$n$ 和 $m$。
- 接下来的$m$行中的第$i$行包含三个整数$a_i$、$d_i$ 和 $k_i$。

保证所有测试用例中的$n$和$m$之和不超过$2 \cdot 10^5$。

### 输出格式

对于每个测试用例，输出连接的组件数量。

---

## 样例 #1

### 样例输入
```
3
10 2
1 2 4
2 2 4
100 1
19 2 4
100 3
1 2 5
7 2 6
17 2 31
```
### 样例输出
```
2
96
61
```
## 提示

在第一个测试用例中，有$n = 10$个点。第一个操作将点$1$、$3$、$5$、$7$和$9$连接在一起。第二个操作将点$2$、$4$、$6$、$8$和$10$连接在一起。因此，有两个连接组件：${1, 3, 5, 7, 9}$和${2, 4, 6, 8, 10}$。

在第二个测试用例中，有$n = 100$个点。唯一的操作将点$19$、$21$、$23$、$25$和$27$连接在一起。它们形成了一个包含5个点的连接组件。其他$95$个点各自形成单点组件。因此答案是$1 + 95 = 96$。

在第三个测试用例中，有$n = 100$个点。所有奇数点从$1$到$79$都将在一个大小为$40$的连接组件中。其余的$60$个点形成单点组件。因此，答案是$1 + 60 = 61$。


```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
void solve() {  
    int n, m;  
    scanf("%d%d", &n, &m);  
    vector<vector<vector<pair<int, int>>>> v(10);  
    for (int i = 0; i < 10; i++) {  
        v[i].resize(i + 1);  
    }  
    while (m--) {  
        int a, d, k;  
        scanf("%d%d%d", &a, &d, &k);  
        v[d - 1][a % d].emplace_back(a, a + k * d);  
    }  
  
    vector<int> p(n + 1), sz(n + 1, 1);  
    iota(p.begin(), p.end(), 0);  
    int cnt = n;  
  
    auto find = [&](auto &&find, int x) -> int {  
        if (p[x] != x) p[x] = find(find, p[x]);  
        return p[x];  
    };  
  
    auto add = [&](int a, int b) {  
        int pa = find(find, a), pb = find(find, b);  
        if (pa != pb) {  
            if (sz[pa] > sz[pb]) swap(pa, pb);  
            sz[pb] += sz[pa];  
            p[pa] = pb;  
            cnt--;  
        }  
    };  
  
    for (int i = 0; i < 10; i++) {  
        for (int j = 0; j <= i; j++) {  
            auto &a = v[i][j];  
            if (a.empty()) continue;  
            sort(a.begin(), a.end());  
            for (int end = a[0].second, u = 0; u < a.size();) {  
                int start = a[u].first;  
                while (u < a.size() && a[u].first <= end) {  
                    end = max(end, a[u].second);  
                    u++;  
                }  
                for (int x = start + i + 1; x <= end; x += i + 1) {  
                    add(start, x);  
                }  
                if (u < a.size()) end = a[u].second;  
            }  
        }  
    }  
  
    printf("%d\n", cnt);  
}  
  
int main() {  
    int testcases;  
    cin >> testcases;  
    while (testcases--) solve();  
    return 0;  
}
```