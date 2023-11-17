## [LeetCode 224. Basic Calculator](https://leetcode.cn/problems/basic-calculator/)

Given a string `s` representing a valid expression, implement a basic calculator to evaluate it, and return *the result of the evaluation*.

**Note:** You are **not** allowed to use any built-in function which evaluates strings as mathematical expressions, such as `eval()`.

**Example 1:**

```
Input: s = "1 + 1"
Output: 2
```

**Example 2:**

```
Input: s = " 2-1 + 2 "
Output: 3
```

**Example 3:**

```
Input: s = "(1+(4+5+2)-3)+(6+8)"
Output: 23
```

**Constraints:**

- `1 <= s.length <= 3 * 105`
- `s` consists of digits, `'+'`, `'-'`, `'('`, `')'`, and `' '`.
- `s` represents a valid expression.
- `'+'` is **not** used as a unary operation (i.e., `"+1"` and `"+(2 + 3)"` is invalid).
- `'-'` could be used as a unary operation (i.e., `"-1"` and `"-(2 + 3)"` is valid).
- There will be no two consecutive operators in the input.
- Every number and running calculation will fit in a signed 32-bit integer.

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
    stack<int> op;
    stack<int> nums;

    bool is_operator(char c) {
        return c == '+' || c == '-';
    }

    void eval() {
        int b = nums.top();
        nums.pop();
        int a = nums.top();
        nums.pop();
        int c = op.top();
        op.pop();
        switch (c) {
            case '+':
                nums.push(a + b);
                break;
            case '-':
                nums.push(a - b);
                break;
            default:
                break;
        }
    }

public:
    int calculate(string s) {
        if (s[0] == '+' || s[0] == '-') s = "0" + s;
        string tmp;
        int n = (int) s.size();
        for (int i = 0; i < n; i++) {
            if (s[i] != ' ') {
                if (s[i] != '-') tmp.push_back(s[i]);
                else {
                    if (s[i - 1] == '(') tmp += "0-";
                    else tmp.push_back(s[i]);
                }
            }
        }
        s = move(tmp);
        n = (int) s.size();
        for (int i = 0; i < n; i++) {
            if (isdigit(s[i])) {
                int x = 0, j = i;
                while (j < n && isdigit(s[j])) {
                    x = x * 10 + (s[j++] - '0');
                }
                nums.push(x);
                i = j - 1;
            } else if (s[i] == '(') {
                op.push(s[i]);
            } else if (s[i] == ')') {
                while (op.top() != '(') {
                    eval();
                }
                op.pop();
            } else if (is_operator(s[i]) && s[i - 1] != '(' && s[i - 1] != '+' && s[i - 1] != '-') {
                while (!op.empty() && op.top() != '(') {
                    eval();
                }
                op.push(s[i]);
            } else if (is_operator(s[i]) && (is_operator(s[i - 1]) || s[i - 1] == '(')) {
                int x = 0, j = i + 1;
                while (j < n && isdigit(s[j])) {
                    x = x * 10 + (s[j++] - '0');
                }
                i = j - 1;
                if (s[i - 1] == '-') {
                    x = -x;
                }
                nums.push(x);
            }
        }
        while (!op.empty()) eval();
        return nums.top();
    }
};
```