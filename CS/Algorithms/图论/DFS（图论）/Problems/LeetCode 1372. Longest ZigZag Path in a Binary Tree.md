## [LeetCode 1372. Longest ZigZag Path in a Binary Tree](https://leetcode.cn/problems/longest-zigzag-path-in-a-binary-tree/)

You are given the `root` of a binary tree.

A ZigZag path for a binary tree is defined as follow:

-   Choose **any** node in the binary tree and a direction (right or left).
-   If the current direction is right, move to the right child of the current node; otherwise, move to the left child.
-   Change the direction from right to left or from left to right.
-   Repeat the second and third steps until you can't move in the tree.

Zigzag length is defined as the number of nodes visited - 1. (A single node has a length of 0).

Return *the longest **ZigZag** path contained in that tree*.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/01/22/sample_1_1702.png)

**Input:** root = \[1,null,1,1,1,null,null,1,1,null,1,null,null,null,1,null,1\]
**Output:** 3
**Explanation:** Longest ZigZag path in blue nodes (right -> left -> right).

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/01/22/sample_2_1702.png)

**Input:** root = \[1,1,1,null,1,null,null,1,1,null,1\]
**Output:** 4
**Explanation:** Longest ZigZag path in blue nodes (left -> right -> left -> right).

**Example 3:**

**Input:** root = \[1\]
**Output:** 0

**Constraints:**

-   The number of nodes in the tree is in the range `[1, 5 * 104]`.
-   `1 <= Node.val <= 100`
```cpp
#include <unordered_map>  
  
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
    int longestZigZag(TreeNode *root) {  
        res = 0;  
        dfs(root);  
        return res;  
    }  
  
private:  
    using PII = pair<int, int>;  
    int res;  
  
    PII dfs(TreeNode *root) {  
        if (!root) return {-1, -1};  
        int l = dfs(root->left).second + 1;  
        int r = dfs(root->right).first + 1;  
        res = max(res, max(l, r));  
        return {l, r};  
    }  
};
```