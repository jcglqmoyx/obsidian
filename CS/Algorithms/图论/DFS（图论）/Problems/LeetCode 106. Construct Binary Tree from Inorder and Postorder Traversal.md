## [LeetCode **106. Construct Binary Tree from Inorder and Postorder Traversal**](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)

Given two integer arrays `inorder` and `postorder` where `inorder` is the inorder traversal of a binary tree and `postorder` is the postorder traversal of the same tree, construct and return *the binary tree*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/02/19/tree.jpg](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
Input: inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
Output: [3,9,20,null,null,15,7]
```

**Example 2:**

```
Input: inorder = [-1], postorder = [-1]
Output: [-1]
```

**Constraints:**

- `1 <= inorder.length <= 3000`
- `postorder.length == inorder.length`
- `3000 <= inorder[i], postorder[i] <= 3000`
- `inorder` and `postorder` consist of **unique** values.
- Each value of `postorder` also appears in `inorder`.
- `inorder` is **guaranteed** to be the inorder traversal of the tree.
- `postorder` is **guaranteed** to be the postorder traversal of the tree.

```jsx
class Solution {
    unordered_map<int, int> pos;
    vector<int> post;
public:
    TreeNode *buildTree(vector<int> &inorder, vector<int> &postorder) {
        int n = (int) inorder.size();
        for (int i = 0; i < n; i++) pos[inorder[i]] = i;
        post = std::move(postorder);
        return construct(0, n - 1, 0, n - 1);
    }

    TreeNode *construct(int il, int ir, int pl, int pr) {
        if (pl > pr) return nullptr;
        auto root = new TreeNode(post[pr]);
        int idx = pos[post[pr]];
        root->left = construct(il, idx - 1, pl, pl + idx - il - 1);
        root->right = construct(idx + 1, ir, pl + idx - il, pr - 1);
        return root;
    }
};
```