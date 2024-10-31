好的，以下是移除了紧挨的 `$` 之间的空格后的 Markdown 内容：

---

# Vessels

## 题面翻译

有$n$个可爱的沙漏，第$i$个沙漏的容量为$a_i$。给定$m$次操作或询问：

①$1\ u\ x$: 在第$u$个沙漏中加容量为$x$的水；

②$2\ u$: 查询在第$u$个沙漏中有多少单位的水。

注意，如果第$i$个沙漏溢出了，**那么溢出的那一部分水会流到第$i+1$个沙漏**；特别的，从最后一层的沙漏漏下来的水会流到地板上。

## 题目描述

There is a system of $n$ vessels arranged one above the other as shown in the figure below. Assume that the vessels are numbered from 1 to $n$, in the order from the highest to the lowest, the volume of the $i$-th vessel is $a_{i}$ liters.

 ![](https://cdn.luogu.com.cn/upload/vjudge_pic/CF371D/f198564e53bb2ea5c99e17b823930a432879118f.png) Initially, all the vessels are empty. In some vessels water is poured. All the water that overflows from the $i$-th vessel goes to the $(i+1)$-th one. The liquid that overflows from the $n$-th vessel spills on the floor.

Your task is to simulate pouring water into the vessels. To do this, you will need to handle two types of queries:

1. Add $x_{i}$ liters of water to the $p_{i}$-th vessel;
2. Print the number of liters of water in the $k_{i}$-th vessel.

When you reply to the second request you can assume that all the water poured up to this point, has already overflown between the vessels.

## 输入格式

The first line contains integer $n$ — the number of vessels ($1<=n<=2·10^{5}$). The second line contains $n$ integers $a_{1},a_{2},...,a_{n}$ — the vessels' capacities ($1<=a_{i}<=10^{9}$). The vessels' capacities do not necessarily increase from the top vessels to the bottom ones (see the second sample). The third line contains integer $m$ — the number of queries ($1<=m<=2·10^{5}$). Each of the next $m$ lines contains the description of one query. The query of the first type is represented as "$1\ p_{i}\ x_{i}$", the query of the second type is represented as "$2\ k_{i}$" ($1<=p_{i}<=n$, $1<=x_{i}<=10^{9}$, $1<=k_{i}<=n$).

## 输出格式

For each query, print on a single line the number of liters of water in the corresponding vessel.

## 样例 #1

### 样例输入 #1

```
2
5 10
6
1 1 4
2 1
1 2 5
1 1 4
2 1
2 2
```

### 样例输出 #1

```
4
5
8
```

## 样例 #2

### 样例输入 #2

```
3
5 10 8
6
1 1 12
2 2
1 1 6
1 3 2
2 2
2 3
```

### 样例输出 #2

```
7
10
5
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 200010;

int n, m;
int ne[N];
int vol[N];
int water[N];

int find(int x) {
    if (ne[x] != x) {
        ne[x] = find(ne[x]);
    }
    return ne[x];
}

void solve() {
   cin >> n;
   for (int i = 1; i <= n; i++) {
       cin >> vol[i];
   }
   iota(ne + 1, ne + n + 1, 1);
   int m;
   cin >> m;
   while (m--) {
       int t, x, p, k;
       cin >> t;
       if (t == 1) {
            cin >> p >> x;
            while (x) {
                int t = min(x, vol[p] - water[p]);
                water[p] += t;
                x -= t;
                if (water[p] == vol[p]) {
                    ne[p] = find(p + 1);
                    p = ne[p];
                }
                if (!p) {
                    break;
                }
            }
       } else {
            cin >> k;
            printf("%d\n", water[k]);
       }
   }
}

int main() {
    solve();
    return 0;
}
```