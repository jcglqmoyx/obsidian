---
page-title: "2458. 移除子树后的二叉树高度 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/height-of-binary-tree-after-subtree-removal-queries/
date: "2023-09-18 11:26:15"
---
You are given the `root` of a **binary tree** with `n` nodes. Each node is assigned a unique value from `1` to `n`. You are also given an array `queries` of size `m`.

You have to perform `m` **independent** queries on the tree where in the `ith` query you do the following:

-   **Remove** the subtree rooted at the node with the value `queries[i]` from the tree. It is **guaranteed** that `queries[i]` will **not** be equal to the value of the root.

Return *an array* `answer` *of size* `m` *where* `answer[i]` *is the height of the tree after performing the* `ith` *query*.

**Note**:

-   The queries are independent, so the tree returns to its **initial** state after each query.
-   The height of a tree is the **number of edges in the longest simple path** from the root to some node in the tree.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/09/07/binaryytreeedrawio-1.png)

**Input:** root = \[1,3,4,2,null,6,5,null,null,null,null,null,7\], queries = \[4\]
**Output:** \[2\]
**Explanation:** The diagram above shows the tree after removing the subtree rooted at node with value 4.
The height of the tree is 2 (The path 1 -> 3 -> 2).

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/09/07/binaryytreeedrawio-2.png)

**Input:** root = \[5,8,9,2,1,3,7,4,6\], queries = \[3,2,4,8\]
**Output:** \[3,2,3,2\]
**Explanation:** We have the following queries:
- Removing the subtree rooted at node with value 3. The height of the tree becomes 3 (The path 5 -> 8 -> 2 -> 4).
- Removing the subtree rooted at node with value 2. The height of the tree becomes 2 (The path 5 -> 8 -> 1).
- Removing the subtree rooted at node with value 4. The height of the tree becomes 3 (The path 5 -> 8 -> 2 -> 6).
- Removing the subtree rooted at node with value 8. The height of the tree becomes 2 (The path 5 -> 9 -> 3).

**Constraints:**

-   The number of nodes in the tree is `n`.
-   `2 <= n <= 105`
-   `1 <= Node.val <= n`
-   All the values in the tree are **unique**.
-   `m == queries.length`
-   `1 <= m <= min(n, 104)`
-   `1 <= queries[i] <= n`
-   `queries[i] != root.val`
```cpp
#include <bits/stdc++.h>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;

    TreeNode() : val(0), left(nullptr), right(nullptr) {}

    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}

    TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
};

const int N = 100010;

int l[N], r[N];

class Solution {
public:
    void dfs1(TreeNode *node, int depth, int &max_depth) {
        if (!node) return;
        l[node->val] = max_depth;
        max_depth = max(max_depth, depth);
        dfs1(node->left, depth + 1, max_depth);
        dfs1(node->right, depth + 1, max_depth);
    }

    void dfs2(TreeNode *node, int depth, int &max_depth) {
        if (!node) return;
        r[node->val] = max_depth;
        max_depth = max(max_depth, depth);
        dfs2(node->right, depth + 1, max_depth);
        dfs2(node->left, depth + 1, max_depth);
    }

    vector<int> treeQueries(TreeNode *root, vector<int> &queries) {
        int max_depth = 0;
        dfs1(root, 0, max_depth);
        max_depth = 0;
        dfs2(root, 0, max_depth);
        int n = (int) queries.size();
        vector<int> res(n);
        for (int i = 0; i < n; i++) {
            res[i] = max(l[queries[i]], r[queries[i]]);
        }
        return res;
    }
};
```