---
page-title: "979. 在二叉树中分配硬币 - 力扣（Leetcode）"
url: https://leetcode.cn/problems/distribute-coins-in-binary-tree/
date: "2023-07-14 10:58:48"
---

> 979\. Distribute Coins in Binary Tree

---

You are given the `root` of a binary tree with `n` nodes where each `node` in the tree has `node.val` coins. There are `n` coins in total throughout the whole tree.

In one move, we may choose two adjacent nodes and move one coin from one node to another. A move may be from parent to child, or from child to parent.

Return *the **minimum** number of moves required to make every node have **exactly** one coin*.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/01/18/tree1.png)

**Input:** root = \[3,0,0\]
**Output:** 2
**Explanation:** From the root of the tree, we move one coin to its left child, and one coin to its right child.

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/01/18/tree2.png)

**Input:** root = \[0,3,0\]
**Output:** 3
**Explanation:** From the left child of the root, we move two coins to the root \[taking two moves\]. Then, we move one coin from the root of the tree to the right child.

**Constraints:**

-   The number of nodes in the tree is `n`.
-   `1 <= n <= 100`
-   `0 <= Node.val <= n`
-   The sum of all `Node.val` is `n`.
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

class Solution {
public:
    int distributeCoins(TreeNode *root) {
        res = 0;
        dfs(root);
        return res;
    }

private:
    int res;

    pair<int, int> dfs(TreeNode *node) {
        if (!node) return {0, 0};
        auto l = dfs(node->left), r = dfs(node->right);
        res += abs(l.first - l.second) + abs(r.first - r.second);
        return {l.first + r.first + node->val, l.second + r.second + 1};
    }
};
```