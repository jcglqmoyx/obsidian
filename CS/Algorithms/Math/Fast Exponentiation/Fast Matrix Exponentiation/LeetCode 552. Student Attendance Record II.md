---
page-title: "Student Attendance Record II - LeetCode"
url: https://leetcode.com/problems/student-attendance-record-ii/description/
date: "2023-11-02 19:41:54"
---
An attendance record for a student can be represented as a string where each character signifies whether the student was absent, late, or present on that day. The record only contains the following three characters:

-   `'A'`: Absent.
-   `'L'`: Late.
-   `'P'`: Present.

Any student is eligible for an attendance award if they meet **both** of the following criteria:

-   The student was absent (`'A'`) for **strictly** fewer than 2 days **total**.
-   The student was **never** late (`'L'`) for 3 or more **consecutive** days.

Given an integer `n`, return *the **number** of possible attendance records of length* `n` *that make a student eligible for an attendance award. The answer may be very large, so return it **modulo*** `109 + 7`.

**Example 1:**

**Input:** n = 2
**Output:** 8
**Explanation:** There are 8 records with length 2 that are eligible for an award:
"PP", "AP", "PA", "LP", "PL", "AL", "LA", "LL"
Only "AA" is not eligible because there are 2 absences (there need to be fewer than 2).

**Example 2:**

**Input:** n = 1
**Output:** 3

**Example 3:**

**Input:** n = 10101
**Output:** 183236316

**Constraints:**

-   `1 <= n <= 105`

DP
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int checkRecord(int n) {
        const int MOD = 1e9 + 7;
        int f[2][3];
        memset(f, 0, sizeof f);
        f[0][0] = 1;
        for (int i = 1; i <= n; i++) {
            int g[2][3];
            memset(g, 0, sizeof g);
            for (int j = 0; j <= 1; j++) {
                for (int k = 0; k <= 2; k++) {
                    g[j][0] = (g[j][0] + f[j][k]) % MOD;
                }
            }
            for (int k = 0; k <= 2; k++) {
                g[1][0] = (g[1][0] + f[0][k]) % MOD;
            }
            for (int j = 0; j <= 1; j++) {
                for (int k = 1; k <= 2; k++) {
                    g[j][k] = (g[j][k] + f[j][k - 1]) % MOD;
                }
            }
            memcpy(f, g, sizeof f);
        }
        int res = 0;
        for (int j = 0; j <= 1; j++) {
            for (int k = 0; k <= 2; k++) {
                res = (res + f[j][k]) % MOD;
            }
        }
        return res;
    }
};
```

Fast exponentiation
```cpp
#include <bits/stdc++.h>

using namespace std;

using LL = long long;

static constexpr int MOD = 1'000'000'007;

vector<vector<LL>> multiply(vector<vector<LL>> a, vector<vector<LL>> b) {
    int rows = a.size(), columns = b[0].size(), temp = b.size();
    vector<vector<LL>> c(rows, vector<LL>(columns));
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < columns; j++) {
            for (int k = 0; k < temp; k++) {
                c[i][j] += a[i][k] * b[k][j];
                c[i][j] %= MOD;
            }
        }
    }
    return c;
}

vector<vector<LL>> pow(vector<vector<LL>> mat, int n) {
    vector<vector<LL>> ret = {{1, 0, 0, 0, 0, 0}};
    while (n > 0) {
        if ((n & 1) == 1) {
            ret = multiply(ret, mat);
        }
        n >>= 1;
        mat = multiply(mat, mat);
    }
    return ret;
}

class Solution {
public:
    int checkRecord(int n) {
        vector<vector<LL>> mat = {{1, 1, 0, 1, 0, 0},
                                  {1, 0, 1, 1, 0, 0},
                                  {1, 0, 0, 1, 0, 0},
                                  {0, 0, 0, 1, 1, 0},
                                  {0, 0, 0, 1, 0, 1},
                                  {0, 0, 0, 1, 0, 0}};
        auto t = pow(mat, n);
        return (int) (accumulate(t[0].begin(), t[0].end(), 0LL) % MOD);
    }
};
```