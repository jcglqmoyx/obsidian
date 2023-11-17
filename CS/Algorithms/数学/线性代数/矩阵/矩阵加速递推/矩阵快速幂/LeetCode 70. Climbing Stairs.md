---
page-title: "70. 爬楼梯 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/climbing-stairs/description/
date: "2023-11-02 09:40:29"
---
You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

**Example 1:**

**Input:** n = 2
**Output:** 2
**Explanation:** There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps

**Example 2:**

**Input:** n = 3
**Output:** 3
**Explanation:** There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

**Constraints:**

-   `1 <= n <= 45`

```cpp
struct Matrix {
    using LL = long long;
    LL a1, a2, b1, b2;

    Matrix(LL _a1, LL _a2, LL _b1, LL _b2) {
        a1 = _a1, a2 = _a2, b1 = _b1, b2 = _b2;
    }

    Matrix operator*(Matrix &m) const {
        return {a1 * m.a1 + a2 * m.b1, a1 * m.a2 + a2 * m.b2, b1 * m.a1 + b2 * m.b1, b1 * m.a2 + b2 * m.b2};
    }
};

Matrix quick_power(Matrix matrix, int n) {
    Matrix res = matrix;
    n--;
    while (n) {
        if (n & 1) res = res * matrix;
        matrix = matrix * matrix;
        n >>= 1;
    }
    return res;
}

class Solution {
public:
    int climbStairs(int n) {
        if (n <= 2) return n;
        auto t = quick_power(Matrix(1, 1, 1, 0), n - 2);
        return t.a1 * 2 + t.a2;
    }
};
```