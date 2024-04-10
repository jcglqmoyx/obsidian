---
page-title: AcWing 1087. 修剪草坪
url: https://www.acwing.com/problem/content/1089/
date: 2024-04-09 18:09:52
---
在一年前赢得了小镇的最佳草坪比赛后，FJ 变得很懒，再也没有修剪过草坪。

现在，新一轮的最佳草坪比赛又开始了，FJ 希望能够再次夺冠。

然而，FJ 的草坪非常脏乱，因此，FJ 只能够让他的奶牛来完成这项工作。

FJ 有 N 只排成一排的奶牛，编号为 1 到 N。

每只奶牛的效率是不同的，奶牛 i 的效率为 Ei。

编号相邻的奶牛们很熟悉，如果 FJ 安排超过 K 只编号连续的奶牛，那么这些奶牛就会罢工去开派对。

因此，现在 FJ 需要你的帮助，找到最合理的安排方案并计算 FJ 可以得到的最大效率。

注意，方案需满足不能包含超过 K 只编号连续的奶牛。

#### 输入格式

第一行：空格隔开的两个整数 N 和 K；

第二到 N+1 行：第 i+1 行有一个整数 Ei。

#### 输出格式

共一行，包含一个数值，表示 FJ 可以得到的最大的效率值。

#### 数据范围

1≤N≤105,  
0≤Ei≤109

#### 输入样例：

```
5 2
1
2
3
4
5
```

#### 输出样例：

```
12
```

#### 样例解释

FJ 有 5 只奶牛，效率分别为 1、2、3、4、5。

FJ 希望选取的奶牛效率总和最大，但是他不能选取超过 2 只连续的奶牛。

因此可以选择第三只以外的其他奶牛，总的效率为 1 + 2 + 4 + 5 = 12。

```cpp
#include <bits/stdc++.h>

using namespace std;

using LL = long long;

const int N = 100010;

int n, k;
int q[N];
LL s[N], f[N];

LL g(int i) {
  return f[i - 1] - s[i];
}

void solve() {
  scanf("%d%d", &n, &k);
  for (int i = 1, x; i <= n; i++) {
    scanf("%d", &x);
    s[i] = s[i - 1] + x;
  }
  int hh = 0, tt = -1;
  q[++tt] = 0;
  for (int i = 1; i <= n; i++) {
    while (i - q[hh] > k) hh++;
    while (hh <= tt && g(i) >= g(q[tt])) tt--;
    f[i] = max(f[i - 1], g(q[hh]) + s[i]);
    q[++tt] = i;
  }
  printf("%lld\n", f[n]);
}

int main() {
  solve();
  return 0;
}
```