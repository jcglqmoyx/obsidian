---
page-title: "1137. 第 N 个泰波那契数 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/n-th-tribonacci-number/
date: "2023-11-02 11:32:13"
---
The Tribonacci sequence Tn is defined as follows: 

T0 = 0, T1 = 1, T2 = 1, and Tn+3 = Tn + Tn+1 + Tn+2 for n >= 0.

Given `n`, return the value of Tn.

**Example 1:**

**Input:** n = 4
**Output:** 4
**Explanation:**
T\_3 = 0 + 1 + 1 = 2
T\_4 = 1 + 1 + 2 = 4

**Example 2:**

**Input:** n = 25
**Output:** 1389537

**Constraints:**

-   `0 <= n <= 37`
-   The answer is guaranteed to fit within a 32-bit integer, ie. `answer <= 2^31 - 1`.

```cpp
#include <bits/stdc++.h>

using namespace std;

using LL = long long;

vector<vector<LL>> times(vector<vector<LL>> &a, vector<vector<LL>> &b) {
    int n1 = (int) a.size(), m1 = (int) a[0].size(), m2 = (int) b[0].size();
    vector<vector<LL>> res(n1, vector<LL>(m2));
    for (int i = 0; i < n1; i++) {
        for (int j = 0; j < m2; j++) {
            for (int u = 0; u < m1; u++) {
                res[i][j] += a[i][u] * b[u][j];
            }
        }
    }
    return res;
}

vector<vector<LL>> quick_power(vector<vector<LL>> v, int n) {
    vector<vector<LL>> res = v;
    n--;
    while (n) {
        if (n & 1) {
            res = times(res, v);
        }
        v = times(v, v);
        n >>= 1;
    }
    return res;
}

class Solution {
public:
    int tribonacci(int n) {
        if (n == 0) return 0;
        if (n <= 2) return 1;
        vector<vector<LL>> matrix = {{1, 1, 0},
                                     {1, 0, 1},
                                     {1, 0, 0}};
        return (int) quick_power(matrix, n - 1)[0][0];
    }
};
```