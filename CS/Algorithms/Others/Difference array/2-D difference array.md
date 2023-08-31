# 2-D difference array

## [AcWing 798](https://www.acwing.com/problem/content/800/)

输入一个 $n$ 行 $m$ 列的整数矩阵，再输入 $q$ 个操作，每个操作包含五个整数 $x_1$, $y_1$, $x_2$, $y_2$, $c$，其中 $(x_1, y_1)$和$(x_2, y_2)$ 表示一个子矩阵的左上角坐标和右下角坐标。

每个操作都要将选中的子矩阵中的每个元素的值加上 $c$。

请你将进行完所有操作后的矩阵输出。

### **输入格式**

第一行包含整数 $n, m, q$。

接下来 $n$ 行，每行包含 $m$ 个整数，表示整数矩阵。

接下来 $q$ 行，每行包含 $5$ 个整数 $x_1$, $y_1$, $x_2$, $y_2$, $c$, 表示一个操作。

### **输出格式**

共 $n$ 行，每行 $m$ 个整数，表示所有操作进行完毕后的最终矩阵。

### **数据范围**

$1≤n,m≤1000$,

$1≤q≤100000$,

$1≤x1≤x2≤n$,

 $1≤y1≤y2≤m$
,

$−1000≤c≤1000$,

-1000 ≤ 矩阵内元素的值 ≤ 1000

### **输入样例：**

```
3 4 3
1 2 2 1
3 2 2 1
1 1 1 1
1 1 2 2 1
1 3 2 3 2
3 1 3 4 1
```

### **输出样例：**

```
2 3 4 1
4 3 4 1
2 2 2 2
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1005;

int a[N][N], diff[N][N];

void insert(int x1, int y1, int x2, int y2, int c) {
    diff[x1][y1] += c;
    diff[x2 + 1][y1] -= c;
    diff[x1][y2 + 1] -= c;
    diff[x2 + 1][y2 + 1] += c;
}

int main() {
    int n, m, q;
    cin >> n >> m >> q;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            cin >> a[i][j];
        }
    }
    while (q--) {
        int x1, y1, x2, y2, c;
        cin >> x1 >> y1 >> x2 >> y2 >> c;
        insert(x1, y1, x2, y2, c);
    }
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            diff[i][j] += diff[i - 1][j] + diff[i][j - 1] - diff[i - 1][j - 1];
            a[i][j] += diff[i][j];
            cout << a[i][j] << ' ';
        }
        cout << endl;
    }
    return 0;
}
```

## [LeetCode 2132. Stamping the Grid](https://leetcode.cn/problems/stamping-the-grid/)

You are given an `m x n` binary matrix `grid` where each cell is either `0` (empty) or `1` (occupied).

You are then given stamps of size `stampHeight x stampWidth`. We want to fit the stamps such that they follow the given **restrictions** and **requirements**:

1. Cover all the **empty** cells.
2. Do not cover any of the **occupied** cells.
3. We can put as **many** stamps as we want.
4. Stamps can **overlap** with each other.
5. Stamps are not allowed to be **rotated**.
6. Stamps must stay completely **inside** the grid.

Return `true` *if it is possible to fit the stamps while following the given restrictions and requirements. Otherwise, return* `false`.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/11/03/ex1.png](https://assets.leetcode.com/uploads/2021/11/03/ex1.png)

```
Input: grid = [[1,0,0,0],[1,0,0,0],[1,0,0,0],[1,0,0,0],[1,0,0,0]], stampHeight = 4, stampWidth = 3
Output: true
Explanation: We have two overlapping stamps (labeled 1 and 2 in the image) that are able to cover all the empty cells.
```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/11/03/ex2.png](https://assets.leetcode.com/uploads/2021/11/03/ex2.png)

```
Input: grid = [[1,0,0,0],[0,1,0,0],[0,0,1,0],[0,0,0,1]], stampHeight = 2, stampWidth = 2
Output: false
Explanation: There is no way to fit the stamps onto all the empty cells without the stamps going outside the grid.
```

**Constraints:**

- `m == grid.length`
- `n == grid[r].length`
- `1 <= m, n <= $10^5$`
- `1 <= m * n <= 2 * $10^5$`
- `grid[r][c]` is either `0` or `1`.
- `1 <= stampHeight, stampWidth <= $10^5$`

```cpp
class Solution {
public:
    bool possibleToStamp(vector<vector<int>> &grid, int h, int w) {
        int n = (int) grid.size(), m = (int) grid[0].size();
        int s[n + 1][m + 1], diff[n + 2][m + 2];
        memset(s, 0, sizeof s), memset(diff, 0, sizeof diff);
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + grid[i - 1][j - 1];
            }
        }
        for (int i = 1; i + h - 1 <= n; i++) {
            for (int j = 1; j + w - 1 <= m; j++) {
                int cnt = s[i + h - 1][j + w - 1] - s[i + h - 1][j - 1] - s[i - 1][j + w - 1] + s[i - 1][j - 1];
                if (!cnt) {
                    diff[i][j] += 1;
                    diff[i][j + w] -= 1;
                    diff[i + h][j] -= 1;
                    diff[i + h][j + w] += 1;
                }
            }
        }
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                diff[i][j] += diff[i - 1][j] + diff[i][j - 1] - diff[i - 1][j - 1];
                if (!grid[i - 1][j - 1]) {
                    if (!diff[i][j]) return false;
                }
            }
        }
        return true;
    }
};
```