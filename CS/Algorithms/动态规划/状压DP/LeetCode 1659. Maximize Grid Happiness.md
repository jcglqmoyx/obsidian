---
page-title: LeetCode 1659. Maximize Grid Happiness
url: https://leetcode.cn/problems/maximize-grid-happiness/description/
date: 2024-09-22 00:45:33
---
You are given four integers, `m`, `n`, `introvertsCount`, and `extrovertsCount`. You have an `m x n` grid, and there are two types of people: introverts and extroverts. There are `introvertsCount` introverts and `extrovertsCount` extroverts.

You should decide how many people you want to live in the grid and assign each of them one grid cell. Note that you **do not** have to have all the people living in the grid.

The **happiness** of each person is calculated as follows:

-   Introverts **start** with `120` happiness and **lose** `30` happiness for each neighbor (introvert or extrovert).
-   Extroverts **start** with `40` happiness and **gain** `20` happiness for each neighbor (introvert or extrovert).

Neighbors live in the directly adjacent cells north, east, south, and west of a person's cell.

The **grid happiness** is the **sum** of each person's happiness. Return *the **maximum possible grid happiness**.*

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/05/grid_happiness.png)

**Input:** m = 2, n = 3, introvertsCount = 1, extrovertsCount = 2
**Output:** 240
**Explanation:** Assume the grid is 1-indexed with coordinates (row, column).
We can put the introvert in cell (1,1) and put the extroverts in cells (1,3) and (2,3).
- Introvert at (1,1) happiness: 120 (starting happiness) - (0 \* 30) (0 neighbors) = 120
- Extrovert at (1,3) happiness: 40 (starting happiness) + (1 \* 20) (1 neighbor) = 60
- Extrovert at (2,3) happiness: 40 (starting happiness) + (1 \* 20) (1 neighbor) = 60
The grid happiness is 120 + 60 + 60 = 240.
The above figure shows the grid in this example with each person's happiness. The introvert stays in the light green cell while the extroverts live on the light purple cells.

**Example 2:**

**Input:** m = 3, n = 1, introvertsCount = 2, extrovertsCount = 1
**Output:** 260
**Explanation:** Place the two introverts in (1,1) and (3,1) and the extrovert at (2,1).
- Introvert at (1,1) happiness: 120 (starting happiness) - (1 \* 30) (1 neighbor) = 90
- Extrovert at (2,1) happiness: 40 (starting happiness) + (2 \* 20) (2 neighbors) = 80
- Introvert at (3,1) happiness: 120 (starting happiness) - (1 \* 30) (1 neighbor) = 90
The grid happiness is 90 + 80 + 90 = 260.

**Example 3:**

**Input:** m = 2, n = 2, introvertsCount = 4, extrovertsCount = 0
**Output:** 240

**Constraints:**

-   `1 <= m, n <= 5`
-   `0 <= introvertsCount, extrovertsCount <= min(m * n, 6)`


三进制状态压缩DP
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int getMaxGridHappiness(int m, int n, int introvertsCount, int extrovertsCount) {  
        if (m < n) swap(m, n);  
        int tot = 1, inf = 100000;  
        for (int i = 0; i < n; i++) tot *= 3;  
  
        int f[m][introvertsCount + 1][extrovertsCount + 1][tot];  
        memset(f, -0x3f, sizeof f);  
  
        int table1[3][3] = {  
                {0,   0,  0},  
                {120, 60, 110},  
                {40,  30, 80},  
        };  
  
        int table2[3][3] = {  
                {0, 0,   0},  
                {0, -60, -10},  
                {0, -10, 40},  
        };  
  
        // 0: no one, 1: introvert,  2: extrovert  
        int row_happiness[tot];  
        memset(row_happiness, 0, sizeof row_happiness);  
        int in[tot], ex[tot];  
        memset(in, 0, sizeof in);  
        memset(ex, 0, sizeof ex);  
        for (int i = 0; i < tot; i++) {  
            int mask = i;  
            for (int prev = 0, j = 0; j < n; j++) {  
                int type = mask % 3;  
                if (type == 1) in[i]++;  
                else if (type == 2) ex[i]++;  
                row_happiness[i] += table1[type][prev];  
                prev = type;  
                mask /= 3;  
            }  
            if (in[i] > introvertsCount || ex[i] > extrovertsCount) row_happiness[i] = -inf;  
            else f[0][in[i]][ex[i]][i] = row_happiness[i];  
        }  
  
        int inter_row_happiness[tot][tot];  
        for (int i = 0; i < tot; i++) {  
            for (int last = 0; last < tot; last++) {  
                if (row_happiness[i] == -inf || row_happiness[last] == -inf) inter_row_happiness[i][last] = -inf;  
                else {  
                    inter_row_happiness[i][last] = row_happiness[i];  
                    if (last) {  
                        int a = i, b = last;  
                        for (int j = 0; j < n; j++) {  
                            int x = a % 3, y = b % 3;  
                            inter_row_happiness[i][last] += table2[x][y];  
                            a /= 3, b /= 3;  
                        }  
                    }  
                }  
            }  
        }  
  
        for (int i = 1; i < m; i++) {  
            for (int a = 0; a <= min(introvertsCount, (i + 1) * n); a++) {  
                for (int b = 0; b <= min(extrovertsCount, (i + 1) * n - a); b++) {  
                    for (int cur = 0; cur < tot; cur++) {  
                        if (a < in[cur] || b < ex[cur]) continue;  
                        int &t = f[i][a][b][cur];  
                        for (int last = 0; last < tot; last++) {  
                            t = max(t, f[i - 1][a - in[cur]][b - ex[cur]][last] + inter_row_happiness[cur][last]);  
                        }  
                    }  
                }  
            }  
        }  
        int res = 0;  
        for (int i = 1; i < tot; i++) {  
            for (int a = 0; a <= min(introvertsCount, m * n); a++) {  
                for (int b = 0; b <= min(extrovertsCount, m * n - a); b++) {  
                    res = max(res, f[m - 1][a][b][i]);  
                }  
            }  
        }  
        return res;  
    }  
};
```


将上面的代码使用滚动数组省去一维
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int getMaxGridHappiness(int m, int n, int introvertsCount, int extrovertsCount) {  
        if (m < n) swap(m, n);  
        int tot = 1, inf = 100000;  
        for (int i = 0; i < n; i++) tot *= 3;  
  
        int f[introvertsCount + 1][extrovertsCount + 1][tot];  
        memset(f, -0x3f, sizeof f);  
  
        int table1[3][3] = {  
                {0,   0,  0},  
                {120, 60, 110},  
                {40,  30, 80},  
        };  
  
        int table2[3][3] = {  
                {0, 0,   0},  
                {0, -60, -10},  
                {0, -10, 40},  
        };  
  
        // 0: no one, 1: introvert,  2: extrovert  
        int row_happiness[tot];  
        memset(row_happiness, 0, sizeof row_happiness);  
        int in[tot], ex[tot];  
        memset(in, 0, sizeof in);  
        memset(ex, 0, sizeof ex);  
        for (int i = 0; i < tot; i++) {  
            int mask = i;  
            for (int prev = 0, j = 0; j < n; j++) {  
                int type = mask % 3;  
                if (type == 1) in[i]++;  
                else if (type == 2) ex[i]++;  
                row_happiness[i] += table1[type][prev];  
                prev = type;  
                mask /= 3;  
            }  
            if (in[i] > introvertsCount || ex[i] > extrovertsCount) row_happiness[i] = -inf;  
            else f[in[i]][ex[i]][i] = row_happiness[i];  
        }  
  
        int inter_row_happiness[tot][tot];  
        for (int i = 0; i < tot; i++) {  
            for (int last = 0; last < tot; last++) {  
                if (row_happiness[i] == -inf || row_happiness[last] == -inf) inter_row_happiness[i][last] = -inf;  
                else {  
                    inter_row_happiness[i][last] = row_happiness[i];  
                    if (last) {  
                        int a = i, b = last;  
                        for (int j = 0; j < n; j++) {  
                            int x = a % 3, y = b % 3;  
                            inter_row_happiness[i][last] += table2[x][y];  
                            a /= 3, b /= 3;  
                        }  
                    }  
                }  
            }  
        }  
  
        for (int i = 1; i < m; i++) {  
            int g[introvertsCount + 1][extrovertsCount + 1][tot];  
            memset(g, -0x3f, sizeof g);  
            for (int a = 0; a <= min(introvertsCount, (i + 1) * n); a++) {  
                for (int b = 0; b <= min(extrovertsCount, (i + 1) * n - a); b++) {  
                    for (int cur = 0; cur < tot; cur++) {  
                        if (a < in[cur] || b < ex[cur]) continue;  
                        int &t = g[a][b][cur];  
                        for (int last = 0; last < tot; last++) {  
                            t = max(t, f[a - in[cur]][b - ex[cur]][last] + inter_row_happiness[cur][last]);  
                        }  
                    }  
                }  
            }  
            memcpy(f, g, sizeof g);  
        }  
        int res = 0;  
        for (int i = 1; i < tot; i++) {  
            for (int a = 0; a <= min(introvertsCount, m * n); a++) {  
                for (int b = 0; b <= min(extrovertsCount, m * n - a); b++) {  
                    res = max(res, f[a][b][i]);  
                }  
            }  
        }  
        return res;  
    }  
};
```

轮廓线DP
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
private:  
    static constexpr int T = 243, N = 5, M = 6;  
    int n, m, tot;  
    int p[N];  
    int d[N * N][T][M + 1][M + 1];  
  
    // 邻居间的分数  
    static constexpr int score[3][3] = {  
            {0, 0,   0},  
            {0, -60, -10},  
            {0, -10, 40}  
    };  
  
public:  
    int getMaxGridHappiness(int m, int n, int introvertsCount, int extrovertsCount) {  
        this->n = n;  
        this->m = m;  
        // 状态总数为 3^n        this->tot = pow(3, n);  
        p[0] = 1;  
        for (int i = 1; i < n; i++) {  
            p[i] = p[i - 1] * 3;  
        }  
  
        // 记忆化搜索数组，初始化为 -1，表示未赋值  
        memset(d, -1, sizeof d);  
        return dfs(0, 0, introvertsCount, extrovertsCount);  
    }  
  
    int dfs(int pos, int mask, int iv, int ev) {  
        if (pos == n * m || (iv == 0 && ev == 0)) {  
            return 0;  
        }  
        int &res = d[pos][mask][iv][ev];  
        if (res != -1) {  
            return res;  
        }  
        res = 0;  
        int up = mask / p[n - 1], left = mask % 3;  
        // 若处于第一列，则左侧没有元素，将 left 置为 0        if (pos % n == 0) {  
            left = 0;  
        }  
        for (int i = 0; i < 3; i++) {  
            if ((i == 1 && iv == 0) || (i == 2 && ev == 0)) {  
                continue;  
            }  
            int next_mask = mask % p[n - 1] * 3 + i;  
            int score_sum = dfs(pos + 1, next_mask, iv - (i == 1), ev - (i == 2))  
                            + score[up][i]  
                            + score[left][i];  
            if (i == 1) {  
                score_sum += 120;  
            } else if (i == 2) {  
                score_sum += 40;  
            }  
            res = max(res, score_sum);  
        }  
        return res;  
    }  
};
```