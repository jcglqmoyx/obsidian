# Evaluating expressions

## [AcWing 3302](https://www.acwing.com/problem/content/3305/)

给定一个表达式，其中运算符仅包含 `+,-,*,/`（加 减 乘 整除），可能包含括号，请你求出表达式的最终值。

**注意：**

- 数据保证给定的表达式合法。
- 题目保证符号 −``只作为减号出现，不会作为负号出现，例如，`1+2`,`(2+2)*(-(1+1)+2)` 之类表达式均不会出现。
- 题目保证表达式中所有数字均为正整数。
- 题目保证表达式在中间计算过程以及结果中，均不超过 $2^{31}-1$。
- 题目中的整除是指向 $0$ 取整，也就是说对于大于 $0$ 的结果向下取整，例如 $5/3=1$，对于小于 $0$的结果向上取整，例如 $5/(1−4)=−1$。
- C++和Java中的整除默认是向零取整；Python中的整除`//`默认向下取整，因此Python的`eval()`函数中的整除也是向下取整，在本题中不能直接使用。

### **输入格式**

共一行，为给定表达式。

### **输出格式**

共一行，为表达式的结果。

### **数据范围**

表达式的长度不超过 $10^5$。

### **输入样例：**

```
(2+2)*(1+1)
```

### **输出样例：**

```
8
```

```cpp
#include <bits/stdc++.h>

using namespace std;

stack<int> nums;
stack<char> op;
unordered_map<char, int> priority{{'+', 1},
                                  {'-', 1},
                                  {'*', 2},
                                  {'/', 2}};

void eval() {
    int b = nums.top();
    nums.pop();
    int a = nums.top();
    nums.pop();
    char c = op.top();
    op.pop();
    int x;
    switch (c) {
        case '+':
            x = a + b;
            break;
        case '-':
            x = a - b;
            break;
        case '*':
            x = a * b;
            break;
        case '/':
            x = a / b;
            break;
        default:
            break;
    }
    nums.push(x);
}

int main() {
    string s;
    cin >> s;
    int n = (int) s.size();
    for (int i = 0; i < n; i++) {
        if (isdigit(s[i])) {
            int x = 0;
            int j = i;
            while (j < n && isdigit(s[j])) {
                x = x * 10 + s[j++] - '0';
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
        } else {
            while (!op.empty() && priority[op.top()] >= priority[s[i]]) {
                eval();
            }
            op.push(s[i]);
        }
    }
    while (!op.empty()) {
        eval();
    }
    cout << nums.top() << endl;
    return 0;
}
```

## [LeetCode 204](https://leetcode.cn/problems/basic-calculator/)

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