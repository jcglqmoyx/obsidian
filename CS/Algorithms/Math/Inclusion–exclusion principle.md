## [AcWing **890. 能被整除的数**](https://www.acwing.com/problem/content/description/892/)

给定一个整数 n 和 m 个不同的质数 $p_1$，$p_2$ , … , $p_m$。

请你求出 1 ∼ n 中能被 $p_1$，$p_2$ , … , $p_m$ 中的至少一个数整除的整数有多少个。

### **输入格式**

第一行包含整数 n 和 m。

第二行包含 m 个质数。

### **输出格式**

输出一个整数，表示满足条件的整数的个数。

### **数据范围**

1 ≤ m ≤ 16，

1 ≤ n, $p_i$≤ $10^9$

### **输入样例：**

```
10 2
2 3
```

### **输出样例：**

```
7
```

```cpp
#include <bits/stdc++.h>

using namespace std;

using LL = long long;

const int N = 20;

int n, m;
int p[N];

int main() {
    cin >> n >> m;
    for (int i = 0; i < m; i++) cin >> p[i];
    int res = 0;
    for (int i = 1; i < 1 << m; i++) {
        int s = 0, t = 1;
        for (int j = 0; j < m; j++) {
            if (i >> j & 1) {
                if ((LL) t * p[j] > n) {
                    t = -1;
                    break;
                }
                s++;
                t *= p[j];
            }
        }
        if (t == -1) continue;
        if (s & 1) res += n / t;
        else res -= n / t;
    }
    cout << res << endl;
    return 0;
}
```

## [LeetCode 2652. Sum Multiples](https://leetcode.cn/problems/sum-multiples/)
Given a positive integer `n`, find the sum of all integers in the range `[1, n]` **inclusive** that are divisible by `3`, `5`, or `7`.

Return *an integer denoting the sum of all numbers in the given range satisfying the constraint.*

**Example 1:**

**Input:** n = 7
**Output:** 21
**Explanation:** Numbers in the range `[1, 7]` that are divisible by `3`, `5,` or `7` are `3, 5, 6, 7`. The sum of these numbers is `21`.

**Example 2:**

**Input:** n = 10
**Output:** 40
**Explanation:** Numbers in the range `[1, 10] that are` divisible by `3`, `5,` or `7` are `3, 5, 6, 7, 9, 10`. The sum of these numbers is 40.

**Example 3:**

**Input:** n = 9
**Output:** 30
**Explanation:** Numbers in the range `[1, 9]` that are divisible by `3`, `5`, or `7` are `3, 5, 6, 7, 9`. The sum of these numbers is `30`.

**Constraints:**

-   `1 <= n <= 1000`
```cpp
class Solution {
public:
    int sumOfMultiples(int n) {
        auto calc = [&](int x) {
            int cnt = n / x;
            int l = x, r = cnt * x;
            return (l + r) * cnt / 2;
        };
        return calc(3) + calc(5) + calc(7) - calc(15) - calc(21) - calc(35) + calc(105);
    }
};
```
