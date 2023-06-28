## **[844. 走迷宫](https://www.acwing.com/problem/content/846/)**

给定一个 $n$*$m$ 的二维整数数组，用来表示一个迷宫，数组中只包含 0 或 1，其中 0 表示可以走的路，11表示不可通过的墙壁。

最初，有一个人位于左上角 (1,1) 处，已知该人每次可以向上、下、左、右任意一个方向移动一个位置。

请问，该人从左上角移动至右下角 (n,m) 处，至少需要移动多少次。

数据保证 (1,1) 处和 (n,m) 处的数字为 0，且一定至少存在一条通路。

### **输入格式**

第一行包含两个整数 n 和 m。

接下来 n 行，每行包含 m 个整数（0 或 1），表示完整的二维数组迷宫。

### **输出格式**

输出一个整数，表示从左上角移动至右下角的最少移动次数。

### **数据范围**

1 ≤ n, m ≤ 100

### **输入样例：**

```
5 5
0 1 0 0 0
0 1 0 1 0
0 0 0 0 0
0 1 1 1 0
0 0 0 1 0
```

### **输出样例：**

```
8
```

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef pair<int, int> PII;

const int N = 105;

bool st[N][N];

int n, m, g[N][N];
int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            scanf("%d", &g[i][j]);
        }
    }
    queue<PII> q;
    q.push({1, 1});
    int dist = 0;
    while (!q.empty()) {
        dist++;
        for (auto it = q.size(); it; it--) {
            auto[x, y] = q.front();
            q.pop();
            for (int i = 0; i < 4; i++) {
                int nx = x + dx[i], ny = y + dy[i];
                if (nx == n && ny == m) {
                    printf("%d\n", dist);
                    exit(0);
                }
                if (nx >= 1 && nx <= n && ny >= 1 && ny <= m && g[nx][ny] != 1 && !st[nx][ny]) {
                    st[nx][ny] = true;
                    q.push({nx, ny});
                }
            }
        }
    }
    return 0;
}
```

## **[Topological sorting](https://www.acwing.com/problem/content/850/)**

给定一个 n 个点 m 条边的有向图，点的编号是 1 到 n，图中可能存在重边和自环。

请输出任意一个该有向图的拓扑序列，如果拓扑序列不存在，则输出 −1。

若一个由图中所有点构成的序列 A 满足：对于图中的每条边 (x, y)，xx在 A 中都出现在 y 之前，则称 A 是该图的一个拓扑序列。

### **输入格式**

第一行包含两个整数 n 和 m。

接下来 m 行，每行包含两个整数 x 和 y，表示存在一条从点 x 到点 y 的有向边 (x, y)。

### **输出格式**

共一行，如果存在拓扑序列，则输出任意一个合法的拓扑序列即可。

否则输出 −1。

### **数据范围**

1 ≤ n, m ≤ $10^5$

### **输入样例：**

```
3 3
1 2
2 3
1 3
```

### **输出样例：**

```
1 2 3
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 100010;

int n, m;
int h[N], e[N], ne[N], idx;
int d[N];
int q[N];

void add(int a, int b) {
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

bool topsort() {
    int hh = 0, tt = -1;

    for (int i = 1; i <= n; i++) {
        if (!d[i]) q[++tt] = i;
    }

    while (hh <= tt) {
        int t = q[hh++];
        for (int i = h[t]; i != -1; i = ne[i]) {
            int j = e[i];
            if (--d[j] == 0) q[++tt] = j;
        }
    }
    return tt == n - 1;
}

int main() {
    scanf("%d%d", &n, &m);

    memset(h, -1, sizeof h);

    for (int i = 0; i < m; i++) {
        int a, b;
        scanf("%d%d", &a, &b);
        add(a, b);
        d[b]++;
    }

    if (!topsort()) puts("-1");
    else {
        for (int i = 0; i < n; i++) printf("%d ", q[i]);
        puts("");
    }
    return 0;
}
```

## [LeetCode 1210. Minimum Moves to Reach Target with Rotations](https://leetcode.cn/problems/minimum-moves-to-reach-target-with-rotations/description/)

In an `n*n` grid, there is a snake that spans 2 cells and starts moving from the top left corner at `(0, 0)` and `(0, 1)`. The grid has empty cells represented by zeros and blocked cells represented by ones. The snake wants to reach the lower right corner at `(n-1, n-2)` and `(n-1, n-1)`.

In one move the snake can:

- Move one cell to the right if there are no blocked cells there. This move keeps the horizontal/vertical position of the snake as it is.
- Move down one cell if there are no blocked cells there. This move keeps the horizontal/vertical position of the snake as it is.
- Rotate clockwise if it's in a horizontal position and the two cells under it are both empty. In that case the snake moves from `(r, c)` and `(r, c+1)` to `(r, c)` and `(r+1, c)`.
    
    ![https://assets.leetcode.com/uploads/2019/09/24/image-2.png](https://assets.leetcode.com/uploads/2019/09/24/image-2.png)
    
- Rotate counterclockwise if it's in a vertical position and the two cells to its right are both empty. In that case the snake moves from `(r, c)` and `(r+1, c)` to `(r, c)` and `(r, c+1)`.
    
    ![https://assets.leetcode.com/uploads/2019/09/24/image-1.png](https://assets.leetcode.com/uploads/2019/09/24/image-1.png)
    

Return the minimum number of moves to reach the target.

If there is no way to reach the target, return `-1`.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/09/24/image.png](https://assets.leetcode.com/uploads/2019/09/24/image.png)

```
Input: grid = [[0,0,0,0,0,1],
               [1,1,0,0,1,0],
               [0,0,0,0,1,1],
               [0,0,1,0,1,0],
               [0,1,1,0,0,0],
               [0,1,1,0,0,0]]
Output: 11
Explanation:
One possible solution is [right, right, rotate clockwise, right, down, down, down, down, rotate counterclockwise, right, down].
```

**Example 2:**

```
Input: grid = [[0,0,1,1,1,1],
               [0,0,0,0,1,1],
               [1,1,0,0,0,1],
               [1,1,1,0,0,1],
               [1,1,1,0,0,1],
               [1,1,1,0,0,0]]
Output: 9
```

**Constraints:**

- `2 <= n <= 100`
- `0 <= grid[i][j] <= 1`
- It is guaranteed that the snake starts at empty cells.

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int minimumMoves(vector<vector<int>> &grid) {
        int n = (int) grid.size(), m = (int) grid[0].size();
        bool st[n][m][2];
        memset(st, 0, sizeof st);
        st[0][0][0] = true;
        queue<tuple<int, int, int>> q;
        q.push({0, 0, 0});
        int step = 0;
        while (!q.empty()) {
            step++;
            for (auto it = q.size(); it; it--) {
                auto[x, y, d] = q.front();
                q.pop();
                if (d == 0) {
                    if (y + 2 < m && !grid[x][y + 2] && !st[x][y + 1][0]) {
                        if (x == n - 1 && y == m - 3) return step;
                        q.push({x, y + 1, 0});
                        st[x][y + 1][0] = true;
                    }
                    if (x + 1 < n && !grid[x + 1][y] && !grid[x + 1][y + 1] && !st[x + 1][y][0]) {
                        if (x == n - 2 && y == m - 2) return step;
                        q.push({x + 1, y, 0});
                        st[x + 1][y][0] = true;
                    }
                    if (x < n - 1 && !grid[x + 1][y] && !grid[x + 1][y + 1] && !st[x][y][1]) {
                        q.push({x, y, 1});
                        st[x][y][1] = true;
                    }
                } else {
                    if (x + 2 < m && !grid[x + 2][y] && !st[x + 1][y][1]) {
                        q.push({x + 1, y, 1});
                        st[x + 1][y][1] = true;
                    }
                    if (y + 1 < m && !grid[x][y + 1] && !grid[x + 1][y + 1] && !st[x][y + 1][1]) {
                        q.push({x, y + 1, 1});
                        st[x][y + 1][1] = true;
                    }
                    if (y + 1 < m && !grid[x][y + 1] && !grid[x + 1][y + 1] && !st[x][y][0]) {
                        q.push({x, y, 0});
                        st[x][y][0] = true;
                    }
                }
            }
        }
        return -1;
    }
};
```