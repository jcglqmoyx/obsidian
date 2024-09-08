---
page-title: LeetCode 3283. Maximum Number of Moves to Kill All Pawns
url: https://leetcode.cn/problems/maximum-number-of-moves-to-kill-all-pawns/description/
date: 2024-09-08 21:03:47
---
There is a `50 x 50` chessboard with **one** knight and some pawns on it. You are given two integers `kx` and `ky` where `(kx, ky)` denotes the position of the knight, and a 2D array `positions` where `positions[i] = [xi, yi]` denotes the position of the pawns on the chessboard.

Alice and Bob play a *turn-based* game, where Alice goes first. In each player's turn:

-   The player *selects* a pawn that still exists on the board and captures it with the knight in the **fewest** possible **moves**. **Note** that the player can select **any** pawn, it **might not** be one that can be captured in the **least** number of moves.
-   In the process of capturing the *selected* pawn, the knight **may** pass other pawns **without** capturing them. **Only** the *selected* pawn can be captured in *this* turn.

Alice is trying to **maximize** the **sum** of the number of moves made by *both* players until there are no more pawns on the board, whereas Bob tries to **minimize** them.

Return the **maximum** *total* number of moves made during the game that Alice can achieve, assuming both players play **optimally**.

Note that in one **move,** a chess knight has eight possible positions it can move to, as illustrated below. Each move is two cells in a cardinal direction, then one cell in an orthogonal direction.

![](https://assets.leetcode.com/uploads/2024/08/01/chess_knight.jpg)

**Example 1:**

**Input:** kx = 1, ky = 1, positions = \[\[0,0\]\]

**Output:** 4

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/08/16/gif3.gif)

The knight takes 4 moves to reach the pawn at `(0, 0)`.

**Example 2:**

**Input:** kx = 0, ky = 2, positions = \[\[1,1\],\[2,2\],\[3,3\]\]

**Output:** 8

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/08/16/gif4.gif)**

-   Alice picks the pawn at `(2, 2)` and captures it in two moves: `(0, 2) -> (1, 4) -> (2, 2)`.
-   Bob picks the pawn at `(3, 3)` and captures it in two moves: `(2, 2) -> (4, 1) -> (3, 3)`.
-   Alice picks the pawn at `(1, 1)` and captures it in four moves: `(3, 3) -> (4, 1) -> (2, 2) -> (0, 3) -> (1, 1)`.

**Example 3:**

**Input:** kx = 0, ky = 0, positions = \[\[1,2\],\[2,4\]\]

**Output:** 3

**Explanation:**

-   Alice picks the pawn at `(2, 4)` and captures it in two moves: `(0, 0) -> (1, 2) -> (2, 4)`. Note that the pawn at `(1, 2)` is not captured.
-   Bob picks the pawn at `(1, 2)` and captures it in one move: `(2, 4) -> (1, 2)`.

**Constraints:**

-   `0 <= kx, ky <= 49`
-   `1 <= positions.length <= 15`
-   `positions[i].length == 2`
-   `0 <= positions[i][0], positions[i][1] <= 49`
-   All `positions[i]` are unique.
-   The input is generated such that `positions[i] != [kx, ky]` for all `0 <= i < positions.length`.

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
    const int N = 50;
    int dx[8] = {2, 1, -1, -2, -2, -1, 1, 2};
    int dy[8] = {1, 2, 2, 1, -1, -2, -2, -1};

    int get_dist(int a, int b, int x, int y) {
        queue<pair<int, int>> q;
        q.emplace(a, b);
        int step = 0;
        bool st[N][N];
        memset(st, 0, sizeof st);
        st[a][b] = true;

        while (!q.empty()) {
            step++;
            for (auto it = q.size(); it; it--) {
                auto t = q.front();
                q.pop();
                int u = t.first, v = t.second;
                if (abs(u - x) + abs(v - y) <= 5) {
                    for (int i = 0; i < 8; i++) {
                        int nu = u + dx[i], nv = v + dy[i];
                        if (nu < 0 || nu >= N || nv < 0 || nv >= N || st[nu][nv]) continue;
                        if (nu == x && nv == y) {
                            return step;
                        }
                        q.emplace(nu, nv);
                        st[nu][nv] = true;
                    }
                } else {
                    for (int i = 0; i < 8; i++) {
                        int nu = u + dx[i], nv = v + dy[i];
                        if (nu < 0 || nu >= N || nv < 0 || nv >= N || st[nu][nv]) continue;
                        if (abs(nu - x) + abs(nv - y) < abs(u - x) + abs(v - y)) {
                            q.emplace(nu, nv);
                            st[nu][nv] = true;
                        }
                    }
                }
            }
        }
        return step;
    }

public:
    int maxMoves(int kx, int ky, vector<vector<int>> &positions) {
        int n = positions.size();
        int dist[n][n];
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                dist[i][j] = dist[j][i] = get_dist(positions[i][0], positions[i][1], positions[j][0], positions[j][1]);
            }
        }
        int f[n][1 << n];
        memset(f, -1, sizeof f);
        function<int(int, int)> dp = [&](int i, int j) -> int {
            int cnt = __builtin_popcount(j);
            if (cnt == 0) return 0;
            if (f[i][j] != -1) return f[i][j];
            if ((cnt & 1) != (n & 1)) {
                f[i][j] = 1e8;
                for (int u = 0; u < n; u++) {
                    if (j >> u & 1) {
                        f[i][j] = min(f[i][j], dp(u, j ^ (1 << u)) + dist[i][u]);
                    }
                }
            } else {
                f[i][j] = 0;
                for (int u = 0; u < n; u++) {
                    if (j >> u & 1) {
                        f[i][j] = max(f[i][j], dp(u, j ^ (1 << u)) + dist[i][u]);
                    }
                }
            }
            return f[i][j];
        };
        int res = 0;
        for (int i = 0; i < n; i++) {
            res = max(res, get_dist(kx, ky, positions[i][0], positions[i][1]) + dp(i, ((1 << n) - 1) ^ (1 << i)));
        }
        return res;
    }
};
```