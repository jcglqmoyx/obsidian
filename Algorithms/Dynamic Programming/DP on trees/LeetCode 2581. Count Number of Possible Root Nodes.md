Alice has an undirected tree with `n` nodes labeled from `0` to `n - 1`. The tree is represented as a 2D integer array `edges` of length `n - 1` where `edges[i] = [$a_i$, $b_i$]` indicates that there is an edge between nodes $`a_i`$ and$`b_i`$ in the tree.

Alice wants Bob to find the root of the tree. She allows Bob to make several **guesses** about her tree. In one guess, he does the following:

- Chooses two **distinct** integers `u` and `v` such that there exists an edge `[u, v]` in the tree.
- He tells Alice that `u` is the **parent** of `v` in the tree.

Bob's guesses are represented by a 2D integer array `guesses` where `guesses[j] = [$u_j$, $v_j$]` indicates Bob guessed$`u_j`$ to be the parent of$`v_j`$.

Alice being lazy, does not reply to each of Bob's guesses, but just says that **at least** `k` of his guesses are `true`.

Given the 2D integer arrays `edges`, `guesses` and the integer `k`, return *the **number of possible nodes** that can be the root of Alice's tree*. If there is no such tree, return `0`.

**Example 1:**

![https://assets.leetcode.com/uploads/2022/12/19/ex-1.png](https://assets.leetcode.com/uploads/2022/12/19/ex-1.png)

```
Input: edges = [[0,1],[1,2],[1,3],[4,2]], guesses = [[1,3],[0,1],[1,0],[2,4]], k = 3
Output: 3
Explanation:
Root = 0, correct guesses = [1,3], [0,1], [2,4]
Root = 1, correct guesses = [1,3], [1,0], [2,4]
Root = 2, correct guesses = [1,3], [1,0], [2,4]
Root = 3, correct guesses = [1,0], [2,4]
Root = 4, correct guesses = [1,3], [1,0]
Considering 0, 1, or 2 as root node leads to 3 correct guesses.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2022/12/19/ex-2.png](https://assets.leetcode.com/uploads/2022/12/19/ex-2.png)

```
Input: edges = [[0,1],[1,2],[2,3],[3,4]], guesses = [[1,0],[3,4],[2,1],[3,2]], k = 1
Output: 5
Explanation:
Root = 0, correct guesses = [3,4]
Root = 1, correct guesses = [1,0], [3,4]
Root = 2, correct guesses = [1,0], [2,1], [3,4]
Root = 3, correct guesses = [1,0], [2,1], [3,2], [3,4]
Root = 4, correct guesses = [1,0], [2,1], [3,2]
Considering any node as root will give at least 1 correct guess.

```

**Constraints:**

- `edges.length == n - 1`
- `2 <= n <= $10^5$`
- `1 <= guesses.length <= $10^5$`
- `0 <= $a_i$, $b_i$, $u_j$, $v_j$ <= n - 1`
- $`a_i$ != $b_i$`
- $`u_j$ != $v_j$`
- `edges` represents a valid tree.
- `guesses[j]` is an edge of the tree.
- `guesses` is unique.
- `0 <= k <= guesses.length`

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1e5 + 5, M = N << 1;

int h[N], e[M], ne[M], idx;
int yes[N], no[N];

void add(int a, int b) {
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

class Solution {
public:
    int rootCount(vector<vector<int>> &edges, vector<vector<int>> &guesses, int k) {
        int n = (int) edges.size() + 1;
        if (!k) return n;
        memset(h, -1, sizeof(int) * n), idx = 0;
        memset(yes, 0, sizeof(int) * n);
        memset(no, 0, sizeof(int) * n);
        for (auto &edge: edges) {
            int x = edge[0], y = edge[1];
            add(x, y), add(y, x);
        }

        unordered_map<int, unordered_set<int>> S;
        for (auto &guess: guesses) {
            int p = guess[0], c = guess[1];
            S[p].insert(c);
        }

        int cnt_right = 0;

        function<void(int, int)> dfs1 = [&](int u, int p) {
            for (int i = h[u]; ~i; i = ne[i]) {
                int j = e[i];
                if (j == p) continue;
                if (S[u].count(j)) {
                    cnt_right++;
                }
                dfs1(j, u);
            }
        };

        dfs1(0, -1);

        int res = cnt_right >= k ? 1 : 0;

        function<void(int, int)> dfs2 = [&](int u, int p) {
            if (p != -1) {
                yes[u] = yes[p], no[u] = no[p];
                if (S[p].count(u)) yes[u]++;
                if (S[u].count(p)) no[u]++;
                if (cnt_right - yes[u] + no[u] >= k) res++;
            }

            for (int i = h[u]; ~i; i = ne[i]) {
                int j = e[i];
                if (j == p) continue;
                dfs2(j, u);
            }
        };

        dfs2(0, -1);
        return res;
    }
};
```
