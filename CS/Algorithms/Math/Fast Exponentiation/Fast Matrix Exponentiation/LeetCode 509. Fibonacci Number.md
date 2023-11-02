---
page-title: "Fibonacci Number - LeetCode"
url: https://leetcode.com/problems/fibonacci-number/description/
date: "2023-11-02 17:36:16"
---

> [509\. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/)

---

The **Fibonacci numbers**, commonly denoted `F(n)` form a sequence, called the **Fibonacci sequence**, such that each number is the sum of the two preceding ones, starting from `0` and `1`. That is,

F(0) = 0, F(1) = 1
F(n) = F(n - 1) + F(n - 2), for n > 1.

Given `n`, calculate `F(n)`.

**Example 1:**

**Input:** n = 2
**Output:** 1
**Explanation:** F(2) = F(1) + F(0) = 1 + 0 = 1.

**Example 2:**

**Input:** n = 3
**Output:** 2
**Explanation:** F(3) = F(2) + F(1) = 1 + 1 = 2.

**Example 3:**

**Input:** n = 4
**Output:** 3
**Explanation:** F(4) = F(3) + F(2) = 2 + 1 = 3.

**Constraints:**

-   `0 <= n <= 30`

```cpp
struct Matrix {
    int a1, a2, b1, b2;

    Matrix(int _a1, int _a2, int _b1, int _b2) {
        a1 = _a1, a2 = _a2, b1 = _b1, b2 = _b2;
    }

    Matrix operator*(Matrix &m) const {
        return {a1 * m.a1 + a2 * m.b1, a1 * m.a2 + a2 * m.b2, b1 * m.a1 + b2 * m.b1, b1 * m.a2 + b2 * m.b2};
    }
};

Matrix quick_power(Matrix matrix, int n) {
    Matrix res(1, 0, 0, 1);
    while (n) {
        if (n & 1) res = res * matrix;
        matrix = matrix * matrix;
        n >>= 1;
    }
    return res;
}

class Solution {
public:
    int fib(int n) {
        if (n <= 1) return n;
        auto t = quick_power(Matrix(1, 1, 1, 0), n - 2);
        return t.a1 + t.a2;
    }
};
```