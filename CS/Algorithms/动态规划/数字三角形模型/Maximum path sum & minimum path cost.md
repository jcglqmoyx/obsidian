## [AcWing **1015. 摘花生**](https://www.acwing.com/problem/content/1017/)

Hello Kitty想摘点花生送给她喜欢的米老鼠。

她来到一片有网格状道路的矩形花生地(如下图)，从西北角进去，东南角出来。

地里每个道路的交叉点上都有种着一株花生苗，上面有若干颗花生，经过一株花生苗就能摘走该它上面所有的花生。

Hello Kitty只能向东或向南走，不能向西或向北走。

问Hello Kitty最多能够摘到多少颗花生。

![https://cdn.acwing.com/media/article/image/2019/09/12/19_a8509f26d5-1.gif](https://cdn.acwing.com/media/article/image/2019/09/12/19_a8509f26d5-1.gif)

### **输入格式**

第一行是一个整数T，代表一共有多少组数据。

接下来是T组数据。

每组数据的第一行是两个整数，分别代表花生苗的行数R和列数 C。

每组数据的接下来R行数据，从北向南依次描述每行花生苗的情况。每行数据有C个整数，按从西向东的顺序描述了该行每株花生苗上的花生数目M。

### **输出格式**

对每组输入数据，输出一行，内容为Hello Kitty能摘到得最多的花生颗数。

### **数据范围**

1 ≤ T ≤ 100,

1 ≤ R, C ≤ 100,

0 ≤ M ≤ 1000

### **输入样例：**

```
2
2 2
1 1
3 4
2 3
2 3 4
1 6 5
```

### **输出样例：**

```
8
16
```

/

DP, 滚动数组，O(C) 空间

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 105;

int f[N];

int main() {
    int t;
    cin >> t;
    while (t--) {
        int r, c;
        scanf("%d%d", &r, &c);
        int num;
        for (int i = 1; i <= r; i++) {
            for (int j = 1; j <= c; j++) {
                scanf("%d", &num);
                f[j] = max(f[j], f[j - 1]) + num;
            }
        }
        printf("%d\n", f[c]);
        memset(f, 0, sizeof f);
    }
    return 0;
}
```

## [AcWing **1027. 方格取数**](https://www.acwing.com/problem/content/1029/)

设有 N × N 的方格图，我们在其中的某些方格中填入正整数，而其它的方格中则放入数字0。如下图所示：

![https://cdn.acwing.com/media/article/image/2019/09/12/19_764ece6ed5-2.gif](https://cdn.acwing.com/media/article/image/2019/09/12/19_764ece6ed5-2.gif)

某人从图中的左上角 A 出发，可以向下行走，也可以向右行走，直到到达右下角的 B 点。

在走过的路上，他可以取走方格中的数（取走后的方格中将变为数字0）。

此人从 A 点到 B 点共走了两次，试找出两条这样的路径，使得取得的数字和为最大。

### **输入格式**

第一行为一个整数N，表示 N × N 的方格图。

接下来的每行有三个整数，第一个为行号数，第二个为列号数，第三个为在该行、该列上所放的数。

行和列编号从 1 开始。

一行“0 0 0”表示结束。

### **输出格式**

输出一个整数，表示两条路径上取得的最大的和。

### **数据范围**

N ≤ 10

### **输入样例：**

```
8
2 3 13
2 6 6
3 5 7
4 4 14
5 2 21
5 6 4
6 3 15
7 2 14
0 0 0
```

### **输出样例：**

```
67
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 15;

int n;
int w[N][N];
int f[2 * N][N][N];

int main() {
    cin >> n;
    int a, b, c;
    while (cin >> a >> b >> c, a || b || c) w[a][b] = c;
    for (int k = 2; k <= 2 * n; k++) {
        for (int i1 = 1; i1 <= n; i1++) {
            for (int i2 = 1; i2 <= n; i2++) {
                int j1 = k - i1, j2 = k - i2;
                if (j1 >= 1 && j1 <= n && j2 >= 1 && j2 <= n) {
                    int t = w[i1][j1];
                    if (i1 != i2) t += w[i2][j2];
                    int &x = f[k][i1][i2];
                    x = max(x, f[k - 1][i1 - 1][i2 - 1] + t);
                    x = max(x, f[k - 1][i1][i2 - 1] + t);
                    x = max(x, f[k - 1][i1 - 1][i2] + t);
                    x = max(x, f[k - 1][i1][i2] + t);
                }
            }
        }
    }
    cout << f[n + n][n][n] << endl;
    return 0;
}
```

## [LeetCode **741. Cherry Pickup**](https://leetcode.cn/problems/cherry-pickup/description/)

You are given an `n x n` `grid` representing a field of cherries, each cell is one of three possible integers.

- `0` means the cell is empty, so you can pass through,
- `1` means the cell contains a cherry that you can pick up and pass through, or
- `-1` means the cell contains a thorn that blocks your way.

Return *the maximum number of cherries you can collect by following the rules below*:

- Starting at the position `(0, 0)` and reaching `(n - 1, n - 1)` by moving right or down through valid path cells (cells with value `0` or `1`).
- After reaching `(n - 1, n - 1)`, returning to `(0, 0)` by moving left or up through valid path cells.
- When passing through a path cell containing a cherry, you pick it up, and the cell becomes an empty cell `0`.
- If there is no valid path between `(0, 0)` and `(n - 1, n - 1)`, then no cherries can be collected.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/12/14/grid.jpg](https://assets.leetcode.com/uploads/2020/12/14/grid.jpg)

```
**Input**: grid = [[0,1,-1],[1,0,-1],[1,1,1]]
**Output**: 5
**Explanation**: The player started at (0, 0) and went down, down, right right to reach (2, 2).
4 cherries were picked up during this single trip, and the matrix becomes [[0,1,-1],[0,0,-1],[0,0,0]].
Then, the player went left, up, up, left to return home, picking up one more cherry.
The total number of cherries picked up is 5, and this is the maximum possible.
```

**Example 2:**

```
**Input**: grid = [[1,1,-1],[1,-1,1],[-1,1,1]]
**Output**: 0
```

**Constraints:**

- `n == grid.length`
- `n == grid[i].length`
- `1 <= n <= 50`
- `grid[i][j]` is `-1`, `0`, or `1`.
- `grid[0][0] != -1`
- `grid[n - 1][n - 1] != -1`

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 55;

int f[N][N][N << 1];

class Solution {
public:
    int cherryPickup(vector<vector<int>> &grid) {
        int n = (int) grid.size(), m = (int) grid[0].size();
        memset(f, -0x3f, sizeof f);
        f[1][1][2] = grid[0][0];
        for (int k = 3; k <= n + m; k++) {
            for (int i = max(1, k - m); i <= min(n, k - 1); i++) {
                for (int j = max(1, k - m); j <= min(n, k - 1); j++) {
                    int r1 = i - 1, c1 = k - i - 1, r2 = j - 1, c2 = k - j - 1;
                    if (grid[r1][c1] == -1 || grid[r2][c2] == -1) continue;
                    int t = grid[r1][c1];
                    if (r1 != r2) t += grid[r2][c2];
                    for (int r = i - 1; r <= i; r++) {
                        for (int c = j - 1; c <= j; c++) {
                            f[i][j][k] = max(f[i][j][k], f[r][c][k - 1]);
                        }
                    }
                    f[i][j][k] += t;
                }
            }
        }
        return max(0, f[n][n][n + m]);
    }
};
```