---
page-title: "1130. 叶值的最小代价生成树 - 力扣（Leetcode）"
url: https://leetcode.cn/problems/minimum-cost-tree-from-leaf-values/
date: "2023-05-31 11:52:13"
---
Given an array `arr` of positive integers, consider all binary trees such that:

-   Each node has either `0` or `2` children;
-   The values of `arr` correspond to the values of each **leaf** in an in-order traversal of the tree.
-   The value of each non-leaf node is equal to the product of the largest leaf value in its left and right subtree, respectively.

Among all possible binary trees considered, return *the smallest possible sum of the values of each non-leaf node*. It is guaranteed this sum fits into a **32-bit** integer.

A node is a **leaf** if and only if it has zero children.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/08/10/tree1.jpg)

**Input:** arr = \[6,2,4\]
**Output:** 32
**Explanation:** There are two possible trees shown.
The first has a non-leaf node sum 36, and the second has non-leaf node sum 32.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/08/10/tree2.jpg)

**Input:** arr = \[4,11\]
**Output:** 44

**Constraints:**

-   `2 <= arr.length <= 40`
-   `1 <= arr[i] <= 15`
-   It is guaranteed that the answer fits into a **32-bit** signed integer (i.e., it is less than 231).
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int mctFromLeafValues(vector<int> &arr) {
        vector<int> stk;
        int res = 0;
        for (int x: arr) {
            if (stk.empty() || x < stk.back()) stk.push_back(x);
            else {
                while (!stk.empty() && x >= stk.back()) {
                    int t = stk.back();
                    stk.pop_back();
                    if (!stk.empty()) res += t * min(stk.back(), x);
                    else res += t * x;
                }
                stk.push_back(x);
            }
        }
        while (stk.size() >= 2) {
            res += stk[stk.size() - 1] * stk[stk.size() - 2];
            stk.pop_back();
        }
        return res;
    }
};
```