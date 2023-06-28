## [LeetCode 236. Lowest Common Ancestor of a Binary Tree](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”

**Example 1:**

![https://assets.leetcode.com/uploads/2018/12/14/binarytree.png](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

**Example 2:**

![https://assets.leetcode.com/uploads/2018/12/14/binarytree.png](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```

**Example 3:**

```
Input: root = [1,2], p = 1, q = 2
Output: 1
```

**Constraints:**

- The number of nodes in the tree is in the range `[2, $10^5$]`.
- $`-10^9$ <= Node.val <= $10^9$`
- All `Node.val` are **unique**.
- `p != q`
- `p` and `q` will exist in the tree.

```cpp
class Solution {
public:
    TreeNode *ans;

    TreeNode *lowestCommonAncestor(TreeNode *root, TreeNode *p, TreeNode *q) {
        dfs(root, p, q);
        return ans;
    }

    bool dfs(TreeNode *root, TreeNode *p, TreeNode *q) {
        if (!root) return false;
        bool l_son = dfs(root->left, p, q);
        bool r_son = dfs(root->right, p, q);
        if (l_son && r_son || (root == p || root == q) && (l_son || r_son)) ans = root;
        return l_son || r_son || root == p || root == q;
    }
};
```

## [LeetCode **1123. Lowest Common Ancestor of Deepest Leaves**](https://leetcode.cn/problems/lowest-common-ancestor-of-deepest-leaves/description/)

Given the `root` of a binary tree, return *the lowest common ancestor of its deepest leaves*.

Recall that:

- The node of a binary tree is a leaf if and only if it has no children
- The depth of the root of the tree is `0`. if the depth of a node is `d`, the depth of each of its children is `d + 1`.
- The lowest common ancestor of a set `S` of nodes, is the node `A` with the largest depth such that every node in `S` is in the subtree with root `A`.

**Example 1:**

![https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/01/sketch1.png](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/01/sketch1.png)

```
**Input**: root = [3,5,1,6,2,0,8,null,null,7,4]
**Output**: [2,7,4]
**Explanation**: We return the node with value 2, colored in yellow in the diagram.
The nodes coloured in blue are the deepest leaf-nodes of the tree.
Note that nodes 6, 0, and 8 are also leaf nodes, but the depth of them is 2, but the depth of nodes 7 and 4 is 3.
```

**Example 2:**

```
**Input**: root = [1]
**Output**: [1]
**Explanation**: The root is the deepest node in the tree, and it's the lca of itself.
```

**Example 3:**

```
**Input**: root = [0,1,3,null,2]
**Output**: [2]
**Explanation**: The deepest leaf node in the tree is 2, the lca of one node is itself.
```

**Constraints:**

- The number of nodes in the tree will be in the range `[1, 1000]`.
- `0 <= Node.val <= 1000`
- The values of the nodes in the tree are **unique**.

### C++ code

```cpp
class Solution {
    /*
     * param node: the root of the tree
     * return: the lowest common ancestor of the deepest leaves of node, the depth of node
     */
    pair<TreeNode *, int> dfs(TreeNode *node) {
        if (!node) return {nullptr, 0};
        auto l = dfs(node->left), r = dfs(node->right);
        if (l.second == r.second) return {node, l.second + 1};
        else if (l.second > r.second) return {l.first, l.second + 1};
        else return {r.first, r.second + 1};
    }

public:
    TreeNode *lcaDeepestLeaves(TreeNode *root) {
        return dfs(root).first;
    }
};
```

### Python3 code

```cpp
class Solution:
    def lcaDeepestLeaves(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        def dfs(node: Optional[TreeNode]):
            if not node:
                return [None, 0]
            left = dfs(node.left)
            right = dfs(node.right)
            if left[1] == right[1]:
                return [node, left[1] + 1]
            elif left[1] > right[1]:
                return [left[0], left[1] + 1]
            else:
                return [right[0], right[1] + 1]

        return dfs(root)[0]
```

## [LeetCode 2581. **Count Number of Possible Root Nodes**](https://leetcode.cn/problems/count-number-of-possible-root-nodes/description/)

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