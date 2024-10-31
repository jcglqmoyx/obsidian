# [ABC357F] Two Sequence Queries

## 题面翻译

给定两个长度为 $N$ 的序列 $A_N, B_N$，现在需要维护 $Q$ 次以下三种操作：

- `1 l r x` 将 $A_l, A_{l + 1}, \cdots, A_r$ 全部加 $x$；
- `2 l r x` 将 $B_l, B_{l + 1}, \cdots, B_r$ 全部加 $x$；
- `3 l r` 求 $\sum_{i = l}^r A_i \times B_i \bmod 998244353$。

## 题目描述

[problemUrl]: https://atcoder.jp/contests/abc357/tasks/abc357_f


## 样例 #1

### 样例输入 #1

```
5 6
1 3 5 6 8
3 1 2 1 2
3 1 3
1 2 5 3
3 1 3
1 1 3 1
2 5 5 2
3 1 5
```

### 样例输出 #1

```
16
25
84
```

## 样例 #2

### 样例输入 #2

```
2 3
1000000000 1000000000
1000000000 1000000000
3 1 1
1 2 2 1000000000
3 1 2
```

### 样例输出 #2

```
716070898
151723988
```


- $1\leq\ N,Q\leq\ 2\times\ 10^5$
- $0\leq\ A_i,B_i\leq\ 10^9$
- $1\leq\ l\leq\ r\leq\ N$
- $1\leq\ x\leq\ 10^9$


```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
using LL = long long;  
  
const int N = 200010;  
const int MOD = 998244353;  
  
int a[N], b[N];  
  
struct Node {  
    int l, r;  
    int s_a;  
    int s_b;  
    int s_mul;  
    int lazy_a;  
    int lazy_b;  
} tr[N << 2];  
  
void push_up(int u) {  
    auto &root = tr[u], &l = tr[u << 1], &r = tr[u << 1 | 1];  
    root.s_a = (l.s_a + r.s_a) % MOD;  
    root.s_b = (l.s_b + r.s_b) % MOD;  
    root.s_mul = (l.s_mul + r.s_mul) % MOD;  
}  
  
void build(int u, int l, int r) {  
    tr[u] = {l, r};  
    if (l == r) {  
        tr[u].s_a = a[l], tr[u].s_b = b[l], tr[u].s_mul = (LL) a[l] * b[l] % MOD;  
    } else {  
        int mid = (l + r) >> 1;  
        build(u << 1, l, mid);  
        build(u << 1 | 1, mid + 1, r);  
        push_up(u);  
    }  
}  
  
void push_down(int u) {  
    auto &root = tr[u], &l = tr[u << 1], &r = tr[u << 1 | 1];  
    if (root.lazy_a || root.lazy_b) {  
        int l_len = l.r - l.l + 1;  
        l.s_mul = (l.s_mul + (LL) root.lazy_a * l.s_b % MOD + (LL) root.lazy_b * l.s_a % MOD + (LL) root.lazy_a * root.lazy_b % MOD * l_len) % MOD;  
        l.s_a = (l.s_a + (LL) root.lazy_a * l_len % MOD) % MOD;  
        l.s_b = (l.s_b + (LL) root.lazy_b * l_len % MOD) % MOD;  
        l.lazy_a = (l.lazy_a + root.lazy_a) % MOD;  
        l.lazy_b = (l.lazy_b + root.lazy_b) % MOD;  
  
        int r_len = r.r - r.l + 1;  
        r.s_mul = (r.s_mul + (LL) root.lazy_a * r.s_b % MOD + (LL) root.lazy_b * r.s_a % MOD + (LL) root.lazy_a * root.lazy_b % MOD * r_len) % MOD;  
        r.s_a = (r.s_a + (LL) root.lazy_a * r_len % MOD) % MOD;  
        r.s_b = (r.s_b + (LL) root.lazy_b * r_len % MOD) % MOD;  
        r.lazy_a = (r.lazy_a + root.lazy_a) % MOD;  
        r.lazy_b = (r.lazy_b + root.lazy_b) % MOD;  
  
        root.lazy_a = 0;  
        root.lazy_b = 0;  
    }  
}  
  
void modify(int u, int l, int r, int da, int db) {  
    if (tr[u].l >= l && tr[u].r <= r) {  
        tr[u].s_a = (tr[u].s_a + (LL) da * (tr[u].r - tr[u].l + 1) % MOD) % MOD;  
        tr[u].s_b = (tr[u].s_b + (LL) db * (tr[u].r - tr[u].l + 1) % MOD) % MOD;  
        tr[u].s_mul = (tr[u].s_mul + (LL) da * tr[u].s_b + (LL) db * tr[u].s_a + (LL) da * db) % MOD;  
        tr[u].lazy_a = (tr[u].lazy_a + da) % MOD;  
        tr[u].lazy_b = (tr[u].lazy_b + db) % MOD;  
    } else {  
        push_down(u);  
        int mid = (tr[u].l + tr[u].r) >> 1;  
        if (l <= mid) modify(u << 1, l, r, da, db);  
        if (r > mid) modify(u << 1 | 1, l, r, da, db);  
        push_up(u);  
    }  
}  
  
int query(int u, int l, int r) {  
    if (tr[u].l >= l && tr[u].r <= r) {  
        return tr[u].s_mul;  
    }  
    push_down(u);  
    int mid = (tr[u].l + tr[u].r) >> 1;  
    int res = 0;  
    if (l <= mid) res = query(u << 1, l, r);  
    if (r > mid) res = (res + query(u << 1 | 1, l, r)) % MOD;  
    push_up(u);  
    return res;  
}  
  
void solve() {  
    int n, q;  
    scanf("%d%d", &n, &q);  
    for (int i = 1; i <= n; i++) {  
        scanf("%d", &a[i]);  
    }  
    for (int i = 1; i <= n; i++) {  
        scanf("%d", &b[i]);  
    }  
    build(1, 1, n);  
    while (q--) {  
        int t, l, r, x;  
        scanf("%d%d%d", &t, &l, &r);  
        if (t == 1) {  
            scanf("%d", &x);  
            modify(1, l, r, x, 0);  
        } else if (t == 2) {  
            scanf("%d", &x);  
            modify(1, l, r, 0, x);  
        } else {  
            printf("%d\n", query(1, l, r));  
        }  
    }  
}  
  
int main() {  
    solve();  
    return 0;  
}
```
