---
page-title: "1449. 数位成本和为目标值的最大数字 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/form-largest-integer-with-digits-that-add-up-to-target/
date: "2023-10-25 19:20:52"
---
Given an array of integers `cost` and an integer `target`, return *the **maximum** integer you can paint under the following rules*:

-   The cost of painting a digit `(i + 1)` is given by `cost[i]` (**0-indexed**).
-   The total cost used must be equal to `target`.
-   The integer does not have `0` digits.

Since the answer may be very large, return it as a string. If there is no way to paint any integer given the condition, return `"0"`.

**Example 1:**

**Input:** cost = \[4,3,2,5,6,7,2,5,5\], target = 9
**Output:** "7772"
**Explanation:** The cost to paint the digit '7' is 2, and the digit '2' is 3. Then cost("7772") = 2\*3+ 3\*1 = 9. You could also paint "977", but "7772" is the largest number.
**Digit    cost**
  1  ->   4
  2  ->   3
  3  ->   2
  4  ->   5
  5  ->   6
  6  ->   7
  7  ->   2
  8  ->   5
  9  ->   5

**Example 2:**

**Input:** cost = \[7,6,5,5,5,6,8,7,8\], target = 12
**Output:** "85"
**Explanation:** The cost to paint the digit '8' is 7, and the digit '5' is 5. Then cost("85") = 7 + 5 = 12.

**Example 3:**

**Input:** cost = \[2,4,6,2,4,6,4,4,4\], target = 5
**Output:** "0"
**Explanation:** It is impossible to paint any integer with total cost equal to target.

**Constraints:**

-   `cost.length == 9`
-   `1 <= cost[i], target <= 5000`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    string largestNumber(vector<int> &cost, int target) {
        if (target < *min_element(cost.begin(), cost.end())) {
            return "0";
        }
        int f[10][target + 1];
        f[0][0] = 0;
        for (int i = 1; i <= target; i++) f[0][i] = -0x3f3f3f3f;
        for (int i = 1; i <= 9; i++) {
            for (int j = 0; j <= target; j++) {
                f[i][j] = f[i - 1][j];
                if (j >= cost[i - 1]) {
                    f[i][j] = max(f[i][j], f[i][j - cost[i - 1]] + 1);
                }
            }
        }
        if (f[9][target] < 1) {
            return "0";
        }
        string res;
        for (int i = 9, j = target; i; i--) {
            while (j >= cost[i - 1] && f[i][j] == f[i][j - cost[i - 1]] + 1) {
                j -= cost[i - 1];
                res.push_back((char) (i + '0'));
            }
        }
        return res;
    }
};
```

空间优化：
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    string largestNumber(vector<int> &cost, int target) {
        if (target < *min_element(cost.begin(), cost.end())) {
            return "0";
        }
        int f[target + 1];
        memset(f, -0x3f, sizeof f);
        f[0] = 0;
        for (int i = 1; i <= 9; i++) {
            for (int j = cost[i - 1]; j <= target; j++) {
                f[j] = max(f[j], f[j - cost[i - 1]] + 1);
            }
        }
        if (f[target] < 1) {
            return "0";
        }
        string res;
        for (int i = 9, j = target; i; i--) {
            while (j >= cost[i - 1] && f[j] == f[j - cost[i - 1]] + 1) {
                j -= cost[i - 1];
                res.push_back((char) (i + '0'));
            }
        }
        return res;
    }
};
```