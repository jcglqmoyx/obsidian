# Minimize the Difference

## 题面翻译

### 题目描述

给你一个长度为 $n$ 的数 $a_1,a_2,…,a_n$ 我们可以对数组进行任意数量(可能是零)的运算。

在每次操作中，我们选择一个位置 $i$ ( $1 \le i \le n−1$) ，使 $a_i-1, a_{i+1}+1$

求 $max(a_1,a_2,…,a_n)−min(a_1,a_2,…,a_n)$ 的最小可能值。

### 输入格式

每个测试包含多个测试用例。第一行包含测试用例的数量 $t$ ( $1 \le t \le 10^5$ ) 。测试用例说明如下:

每个测试用例的第一行都包含一个整数 $n$ ( $1 \le n \le 2 \times 10^{5}$ ) 。

每个测试用例的第二行包含 $n$ 个整数 $a_1,a_2,…,a_n$ ( $1 \le a_i \le 10^{12}$ ) 。

所有测试用例中 $n$
 的总和不超过 $2 \times 10^{5}$ 。
 
###  输出格式

对于每个测试用例，输出一个整数 $ans$ 表示 $max(a_1,a_2,…,a_n)−min(a_1,a_2,…,a_n)$
 的最小可能值。
 
###  提示/说明

在第三个测试案例中，您可以使用 $i=1$
 执行两次操作。
之后，数组为 $[2,3,2,3]$
 所以 $ans=max(2,3,2,3)−min(2,3,2,3)=3−2=1$
 。

## 题目描述

Zhan, tired after the contest, gave the only task that he did not solve during the contest to his friend, Sungat. However, he could not solve it either, so we ask you to try to solve this problem.

You are given an array $a_1, a_2, \ldots, a_n$ of length $n$ . We can perform any number (possibly, zero) of operations on the array.

In one operation, we choose a position $i$ ( $1 \leq i \leq n - 1$ ) and perform the following action:

- $a_i := a_i - 1$ , and $a_{i+1} := a_{i+1} + 1$ .

Find the minimum possible value of $ \max(a_1, a_2, \ldots, a_n) - \min(a_1, a_2, \ldots, a_n) $ .

## 输入格式

Each test contains multiple test cases. The first line contains the number of test cases $t$ ( $1 \le t \le 10^5$ ). The description of the test cases follows.

The first line of each test case contains an integer $n$ ( $1 \leq n \leq 2 \cdot 10^5$ ).

The second line of each test case contains $n$ integers $a_1, a_2, \ldots, a_n$ ( $1 \leq a_i \leq 10^{12}$ ).

The sum of $n$ over all test cases does not exceed $2 \cdot 10^5$ .

## 输出格式

For each test case, output a single integer: the minimum possible value of $\max(a_1, a_2, \ldots, a_n) - \min(a_1, a_2, \ldots, a_n)$ .

## 样例 #1

### 样例输入 #1

```
5
1
1
3
1 2 3
4
4 1 2 3
4
4 2 3 1
5
5 14 4 10 2
```

### 样例输出 #1

```
0
2
1
1
3
```

## 提示

In the third testcase, you can perform the operation twice with $i = 1$ .

After that, the array is $a = [2, 3, 2, 3]$ , and $\max(2, 3, 2, 3) - \min(2, 3, 2, 3) = 3 - 2 = 1$ .


二分
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
using LL = long long;  
  
const int N = 200010;  
  
int n;  
LL a[N], b[N];  
  
bool check1(LL mid) {  
    memcpy(b, a, sizeof(LL) * n);  
    for (int i = 0; i < n; i++) {  
        if (b[i] > mid) {  
            if (i == n - 1) return false;  
            b[i + 1] += b[i] - mid;  
        }  
    }  
    return true;  
}  
  
bool check2(LL mid) {  
    LL cnt = 0;  
    for (int i = n - 1; i >= 0; i--) {  
        if (a[i] < mid) cnt += mid - a[i];  
        else if (a[i] > mid) cnt -= min(cnt, a[i] - mid);  
    }  
    return cnt <= 0;  
}  
  
void solve() {  
    scanf("%d", &n);  
    for (int i = 0; i < n; i++) {  
        scanf("%lld", &a[i]);  
    }  
  
    LL mn = *min_element(a, a + n);  
    LL l = mn, r = *max_element(a, a + n);  
    while (l < r) {  
        LL mid = (l + r) >> 1;  
        if (check1(mid)) r = mid;  
        else l = mid + 1;  
    }  
    LL mx = l;  
    l = mn, r = mx;  
    while (l < r) {  
        LL mid = (l + r + 1) >> 1;  
        if (check2(mid)) l = mid;  
        else r = mid - 1;  
    }  
    printf("%lld\n", mx - l);  
}  
  
int main() {  
    int testcases;  
    scanf("%d", &testcases);  
    while (testcases--) solve();  
    return 0;  
}
```

数学
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
using LL = long long;  
  
const int N = 200010;  
  
int n;  
LL a[N];  
  
void solve() {  
    scanf("%d", &n);  
    for (int i = 0; i < n; i++) {  
        scanf("%lld", &a[i]);  
    }  
    LL mn = 1e18, mx = -1e18;  
    LL s = 0;  
    for (int i = 0; i < n; i++) {  
        s += a[i];  
        mn = min(mn, s / (i + 1));  
    }  
    s = 0;  
    for (int i = n - 1; i >= 0; i--) {  
        s += a[i];  
        mx = max(mx, (s + n - i - 1) / (n - i));  
    }  
    printf("%lld\n", mx - mn);  
}  
  
int main() {  
    int testcases;  
    scanf("%d", &testcases);  
    while (testcases--) solve();  
    return 0;  
}
```