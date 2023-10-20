## [LeetCode **297. Serialize and Deserialize Binary Tree**](https://leetcode.cn/problems/serialize-and-deserialize-binary-tree/description/)

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

**Clarification:** The input/output format is the same as [how LeetCode serializes a binary tree](https://support.leetcode.com/hc/en-us/articles/360011883654-What-does-1-null-2-3-mean-in-binary-tree-representation-). You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/09/15/serdeser.jpg](https://assets.leetcode.com/uploads/2020/09/15/serdeser.jpg)

```
Input: root = [1,2,3,null,null,4,5]
Output: [1,2,3,null,null,4,5]
```

**Example 2:**

```
Input: root = []
Output: []
```

**Constraints:**

- The number of nodes in the tree is in the range `[0, $10^4$]`.
- `1000 <= Node.val <= 1000`

```cpp
#include <bits/stdc++.h>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;

    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class Codec {
public:
    string serialize(TreeNode *root) {
        string res;
        dfs_s(root, res);
        return res;
    }

    TreeNode *deserialize(string data) {
        int index = 0;
        return dfs_d(data, index);
    }

private:
    void dfs_s(TreeNode *root, string &res) {
        if (!root) {
            res += "null ";
            return;
        }
        res += to_string(root->val) + ' ';
        dfs_s(root->left, res);
        dfs_s(root->right, res);
    }

    TreeNode *dfs_d(string &data, int &index) {
        if (index == data.length()) return nullptr;
        int k = index;
        while (k < data.length() && data[k] != ' ') {
            k++;
        }
        if (data[index] == 'n') {
            index = k + 1;
            return nullptr;
        }
        int val = stoi(data.substr(index, k - index));
        index = k + 1;
        auto *root = new TreeNode(val);
        root->left = dfs_d(data, index);
        root->right = dfs_d(data, index);
        return root;
    }
};
```