## [LeetCode 382. Linked List Random Node](https://leetcode.cn/problems/linked-list-random-node/description/)

Given a singly linked list, return a random node's value from the linked list. Each node must have the **same probability** of being chosen.

Implement the `Solution` class:

- `Solution(ListNode head)` Initializes the object with the head of the singly-linked list `head`.
- `int getRandom()` Chooses a node randomly from the list and returns its value. All the nodes of the list should be equally likely to be chosen.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/03/16/getrand-linked-list.jpg](https://assets.leetcode.com/uploads/2021/03/16/getrand-linked-list.jpg)

```
Input
["Solution", "getRandom", "getRandom", "getRandom", "getRandom", "getRandom"]
[[[1, 2, 3]], [], [], [], [], []]
Output
[null, 1, 3, 2, 2, 3]

Explanation
Solution solution = new Solution([1, 2, 3]);
solution.getRandom(); // return 1
solution.getRandom(); // return 3
solution.getRandom(); // return 2
solution.getRandom(); // return 2
solution.getRandom(); // return 3
// getRandom() should return either 1, 2, or 3 randomly. Each element should have equal probability of returning.
```

**Constraints:**

- The number of nodes in the linked list will be in the range `[1, 10000]`.
- `-10000 <= Node.val <= 10000`
- At most `104` calls will be made to `getRandom`.

**Follow up:**

- What if the linked list is extremely large and its length is unknown to you?
- Could you solve this efficiently without using extra space?

```cpp
#include <bits/stdc++.h>

using namespace std;

struct ListNode {
    int val;
    ListNode *next;

    ListNode() : val(0), next(nullptr) {}

    ListNode(int x) : val(x), next(nullptr) {}

    ListNode(int x, ListNode *next) : val(x), next(next) {}
};

class Solution {
    ListNode *p;

public:
    Solution(ListNode *head) {
        p = head;
    }

    int getRandom() {
        ListNode *temp = p;
        int count = 1;
        int ans = 0;
        while (temp) {
            if (rand() % count == 0) {
                ans = temp->val;
            }
            count++;
            temp = temp->next;
        }
        return ans;
    }
};
```