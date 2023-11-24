---
page-title: "255. 第K小数 - AcWing题库"
url: https://www.acwing.com/problem/content/257/
date: "2023-11-24 20:05:40"
---
255\. 第K小数

-     [题目](https://www.acwing.com/problem/content/description/257/)
-     [提交记录](https://www.acwing.com/problem/content/submission/257/)
-     [讨论](https://www.acwing.com/problem/content/discussion/index/257/1/)
-     [题解](https://www.acwing.com/problem/content/solution/257/1/)
-     [视频讲解](https://www.acwing.com/problem/content/video/257/)

  

给定长度为 N 的整数序列 A，下标为 1∼N。

现在要执行 M 次操作，其中第 i 次操作为给出三个整数 li,ri,ki，求 A\[li\],A\[li+1\],…,A\[ri\] (即 A 的下标区间 \[li,ri\])中第 ki 小的数是多少。

#### 输入格式

第一行包含两个整数 N 和 M。

第二行包含 N 个整数，表示整数序列 A。

接下来 M 行，每行包含三个整数 li,ri,ki，用以描述第 i 次操作。

#### 输出格式

对于每次操作输出一个结果，表示在该次操作中，第 k 小的数的数值。

每个结果占一行。

#### 数据范围

N≤105,M≤104,|A\[i\]|≤109

#### 输入样例：

```
7 3
1 5 2 6 3 7 4
2 5 3
4 4 1
1 7 3
```

#### 输出样例：

```
5
6
3
```

---

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 100010, M = N * 21;

int n, m;
int a[N];
vector<int> v;

int root[N], lc[M], rc[M], cnt[M], idx;

int get(int x) {
    return lower_bound(v.begin(), v.end(), x) - v.begin();
}

int push_up(int u) {
    cnt[u] = cnt[lc[u]] + cnt[rc[u]];
}

int build(int l, int r) {
    int p = ++idx;
    if (l == r) return p;
    int mid = l + r >> 1;
    lc[p] = build(l, mid), rc[p] = build(mid + 1, r);
    return p;
}

int insert(int p, int l, int r, int x) {
    int q = ++idx;
    lc[q] = lc[p], rc[q] = rc[p], cnt[q] = cnt[p] + 1;
    if (l < r) {
        int mid = l + r >> 1;
        if (x <= mid) lc[q] = insert(lc[p], l, mid, x);
        else rc[q] = insert(rc[p], mid + 1, r, x);
    }
    return q;
}

int query(int q, int p, int l, int r, int k) {
    if (l == r) return l;
    int tot = cnt[lc[q]] - cnt[lc[p]];
    int mid = l + r >> 1;
    if (k <= tot) return query(lc[q], lc[p], l, mid, k);
    return query(rc[q], rc[p], mid + 1, r, k - tot);
}

void solve() {
    v.push_back(INT32_MIN);
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i++) {
        scanf("%d", &a[i]);
        v.push_back(a[i]);
    }
    sort(v.begin(), v.end());
    v.erase(unique(v.begin(), v.end()), v.end());

    root[0] = build(1, v.size());
    for (int i = 1; i <= n; i++) {
        root[i] = insert(root[i - 1], 1, v.size(), get(a[i]));
    }
    while (m--) {
        int l, r, k;
        scanf("%d%d%d", &l, &r, &k);
        printf("%d\n", v[query(root[r], root[l - 1], 1, v.size(), k)]);
    }
}

int main() {
    solve();
    return 0;
}
```