https://www.luogu.com.cn/problem/CF1730B

## 题面翻译

给定长为 $n$ 的序列 $x$ 和 $t$，请你求出一个 $x_0$，使得 $\max\limits_{i=1}^{n}\{t_i+|x_i-x_0|\}$ 最小，输出 $x_0$。

## 题目描述

$ n $ people live on the coordinate line, the $ i $ -th one lives at the point $ x_i $ ( $ 1 \le i \le n $ ). They want to choose a position $ x_0 $ to meet. The $ i $ -th person will spend $ |x_i - x_0| $ minutes to get to the meeting place. Also, the $ i $ -th person needs $ t_i $ minutes to get dressed, so in total he or she needs $ t_i + |x_i - x_0| $ minutes.

Here $ |y| $ denotes the absolute value of $ y $ .

These people ask you to find a position $ x_0 $ that minimizes the time in which all $ n $ people can gather at the meeting place.

## 输入格式

The first line contains a single integer $ t $ ( $ 1 \le t \le 10^3 $ ) — the number of test cases. Then the test cases follow.

Each test case consists of three lines.

The first line contains a single integer $ n $ ( $ 1 \le n \le 10^5 $ ) — the number of people.

The second line contains $ n $ integers $ x_1, x_2, \dots, x_n $ ( $ 0 \le x_i \le 10^{8} $ ) — the positions of the people.

The third line contains $ n $ integers $ t_1, t_2, \dots, t_n $ ( $ 0 \le t_i \le 10^{8} $ ), where $ t_i $ is the time $ i $ -th person needs to get dressed.

It is guaranteed that the sum of $ n $ over all test cases does not exceed $ 2 \cdot 10^5 $ .

## 输出格式

For each test case, print a single real number — the optimum position $ x_0 $ . It can be shown that the optimal position $ x_0 $ is unique.

Your answer will be considered correct if its absolute or relative error does not exceed $ 10^{−6} $ . Formally, let your answer be $ a $ , the jury's answer be $ b $ . Your answer will be considered correct if $ \frac{|a−b|}{max(1,|b|)} \le 10^{−6} $ .

## 样例 #1

### 样例输入 #1

```
7
1
0
3
2
3 1
0 0
2
1 4
0 0
3
1 2 3
0 0 0
3
1 2 3
4 1 2
3
3 3 3
5 3 3
6
5 4 7 2 10 4
3 2 5 1 4 6
```

### 样例输出 #1

```
0
2
2.5
2
1
3
6
```

## 提示

- In the $ 1 $ -st test case there is one person, so it is efficient to choose his or her position for the meeting place. Then he or she will get to it in $ 3 $ minutes, that he or she need to get dressed.
- In the $ 2 $ -nd test case there are $ 2 $ people who don't need time to get dressed. Each of them needs one minute to get to position $ 2 $ .
- In the $ 5 $ -th test case the $ 1 $ -st person needs $ 4 $ minutes to get to position $ 1 $ ( $ 4 $ minutes to get dressed and $ 0 $ minutes on the way); the $ 2 $ -nd person needs $ 2 $ minutes to get to position $ 1 $ ( $ 1 $ minute to get dressed and $ 1 $ minute on the way); the $ 3 $ -rd person needs $ 4 $ minutes to get to position $ 1 $ ( $ 2 $ minutes to get dressed and $ 2 $ minutes on the way).

|方法一：二分最短时间  <br>二分时间，可以知道每个人的活动范围。  <br>如果这些范围的交集不为空，那么集合地点可以取交集中任意一点。  <br>在二分的同时，记录交集左端点即可。  <br>[代码一](https://codeforces.com/contest/1730/submission/236868467)  <br>  <br>方法二  <br>所有 t[i] 都为 0，应该选在哪里集合？  <br>我们可以把一个人拆成两个人，分别在 x[i]-t[i] 和 x[i]+t[i]，且这两个人的 t 值均为 0。  <br>那么答案就是这些数的 (最小值+最大值)/2  <br>[代码二](https://codeforces.com/contest/1730/submission/236875311)|

方法二 C++代码：
```cpp
#include <bits/stdc++.h>

using namespace std;

using LL = long long;
using VI = vector<int>;
using PII = pair<int, int>;

#define rep(i, a, b) for (int (i) = (a); (i) < (b); (i)++)
#define per(i, a, b) for (int (i) = (a); (i) > (b); (i)--)

const int N = 100010;

int n;
int x[N], t[N];

void solve() {
    scanf("%d", &n);
    rep(i, 0, n) scanf("%d", &x[i]); 
    rep(i, 0, n) scanf("%d", &t[i]); 
    int mn = 1e9, mx = -mn;
    rep(i, 0, n) {
        mn = min(mn, x[i] - t[i]);
        mx = max(mx, x[i] + t[i]);
    }
    printf("%f\n", (mn + mx) / 2.0);
}

int main() {
    int testcases;
    scanf("%d", &testcases);
    while (testcases--) solve();
    return 0;
}
```