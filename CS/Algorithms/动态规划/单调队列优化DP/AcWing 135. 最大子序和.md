---
page-title: AcWing 135. 最大子序和
url: https://www.acwing.com/problem/content/137/
date: 2024-04-09 17:59:06
---
输入一个长度为 n 的整数序列，从中找出一段长度不超过 m 的连续子序列，使得子序列中所有数的和最大。

**注意：** 子序列的长度至少是 1。

#### 输入格式

第一行输入两个整数 n,m。

第二行输入 n 个数，代表长度为 n 的整数序列。

同一行数之间用空格隔开。

#### 输出格式

输出一个整数，代表该序列的最大子序和。

#### 数据范围

1≤n,m≤300000,  
保证所有输入和最终结果都在 int 范围内。

#### 输入样例：

```
6 4
1 -3 5 1 -2 3
```

#### 输出样例：

```
7
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 300010;

int n, m;
int s[N], q[N];

void solve() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i++) {
        int x;
        scanf("%d", &x);
        s[i] = s[i - 1] + x;
    }
    int hh = 0, tt = -1;
    q[++tt] = 0;
    int res = INT32_MIN;
    for (int i = 1; i <= n; i++) {
        while (i - q[hh] > m) hh++;
        res = max(res, s[i] - s[q[hh]]);
        while (hh <= tt && s[i] <= s[q[tt]]) tt--;
        q[++tt] = i;
    }
    printf("%d\n", res);
}

int main() {
    solve();
    return 0;
}
```