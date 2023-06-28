---
page-title: "2719. 统计整数数目 - 力扣（Leetcode）"
url: https://leetcode.cn/problems/count-of-integers/description/
date: "2023-06-06 10:05:08"
---
You are given two numeric strings `num1` and `num2` and two integers `max_sum` and `min_sum`. We denote an integer `x` to be *good* if:

-   `num1 <= x <= num2`
-   `min_sum <= digit_sum(x) <= max_sum`.

Return *the number of good integers*. Since the answer may be large, return it modulo `109 + 7`.

Note that `digit_sum(x)` denotes the sum of the digits of `x`.

**Example 1:**

**Input:** num1 = "1", num2 = "12", min\_num = 1, max\_num = 8
**Output:** 11
**Explanation:** There are 11 integers whose sum of digits lies between 1 and 8 are 1,2,3,4,5,6,7,8,10,11, and 12. Thus, we return 11.

**Example 2:**

**Input:** num1 = "1", num2 = "5", min\_num = 1, max\_num = 5
**Output:** 5
**Explanation:** The 5 integers whose sum of digits lies between 1 and 5 are 1,2,3,4, and 5. Thus, we return 5.

**Constraints:**

-   `1 <= num1 <= num2 <= 1e22`
-   `1 <= min_sum <= max_sum <= 400`
```cpp
#include <bits/stdc++.h>

using namespace std;

const int MOD = 1e9 + 7;

int get(string &s, int min_sum, int max_sum) {
    int n = (int) s.size();
    int f[n][min(n * 9, max_sum) + 1];
    memset(f, -1, sizeof f);
    function<int(int, int, bool)> dp = [&](int u, int sum, bool is_limited) -> int {
        if (sum > max_sum) return 0;
        if (u == n) return sum >= min_sum;
        if (!is_limited && f[u][sum] != -1) return f[u][sum];
        int res = 0, up = is_limited ? s[u] - '0' : 9;
        for (int i = 0; i <= up; i++) {
            res = (res + dp(u + 1, sum + i, is_limited && i == up)) % MOD;
        }
        if (!is_limited) f[u][sum] = res;
        return res;
    };
    return dp(0, 0, true);
}

class Solution {
public:
    int count(string num1, string num2, int min_sum, int max_sum) {
        int res = get(num2, min_sum, max_sum) - get(num1, min_sum, max_sum) + MOD;
        int sum = 0;
        for (char c: num1) sum += c - '0';
        res += sum >= min_sum && sum <= max_sum;
        return res % MOD;
    }
};
```