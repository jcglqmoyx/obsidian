## [1803. Count Pairs With XOR in a Range](https://leetcode.cn/problems/count-pairs-with-xor-in-a-range/)

Given a **(0-indexed)** integer array `nums` and two integers `low` and `high`, return *the number of **nice pairs***.

A **nice pair** is a pair `(i, j)` where `0 <= i < j < nums.length` and `low <= (nums[i] XOR nums[j]) <= high`.

**Example 1:**

```
Input: nums = [1,4,2,7], low = 2, high = 6
Output: 6
Explanation: All nice pairs (i, j) are as follows:
    - (0, 1): nums[0] XOR nums[1] = 5
    - (0, 2): nums[0] XOR nums[2] = 3
    - (0, 3): nums[0] XOR nums[3] = 6
    - (1, 2): nums[1] XOR nums[2] = 6
    - (1, 3): nums[1] XOR nums[3] = 3
    - (2, 3): nums[2] XOR nums[3] = 5
```

**Example 2:**

```
Input: nums = [9,8,4,2,1], low = 5, high = 14
Output: 8
Explanation: All nice pairs (i, j) are as follows:
    - (0, 2): nums[0] XOR nums[2] = 13
    - (0, 3): nums[0] XOR nums[3] = 11
    - (0, 4): nums[0] XOR nums[4] = 8
    - (1, 2): nums[1] XOR nums[2] = 12
    - (1, 3): nums[1] XOR nums[3] = 10
    - (1, 4): nums[1] XOR nums[4] = 9
    - (2, 3): nums[2] XOR nums[3] = 6
    - (2, 4): nums[2] XOR nums[4] = 5
```

**Constraints:**

- `1 <= nums.length <= 2 * $10^4$`
- `1 <= nums[i] <= 2 * $2*10^4$`
- `1 <= low <= high <= $2*10^4$`

### Using struct

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
    struct Trie {
        int cnt;
        Trie *ne[2];

        Trie() : cnt(0), ne() {}

        void insert(int x) {
            auto node = this;
            for (int i = 14; i >= 0; i--) {
                int j = x >> i & 1;
                if (!node->ne[j]) node->ne[j] = new Trie();
                node = node->ne[j];
                node->cnt++;
            }
        }

        int query(int x, int limit) {
            int res = 0;
            auto node = this;
            for (int i = 14; i >= 0; i--) {
                if (!node) return res;
                int j = x >> i & 1;
                int k = limit >> i & 1;
                if (k) {
                    if (node->ne[j]) res += node->ne[j]->cnt;
                    node = node->ne[1 ^ j];
                } else {
                    node = node->ne[j];
                }
            }
            if (node) res += node->cnt;
            return res;
        }
    };

public:
    int countPairs(vector<int> &nums, int low, int high) {
        auto trie = new Trie();
        int res = 0;
        for (int x: nums) {
            res += trie->query(x, high) - trie->query(x, low - 1);
            trie->insert(x);
        }
        return res;
    }
};
```

### Using array to simulate Trie

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 20010, M = N * 15;

int son[M][2], idx;
int cnt[M];

void insert(int x) {
    int p = 0;
    for (int i = 14; i >= 0; i--) {
        int j = x >> i & 1;
        if (!son[p][j]) son[p][j] = ++idx;
        p = son[p][j];
        cnt[p]++;
    }
}

int query(int num, int limit) {
    int res = 0;
    int p = 0;
    for (int i = 14; i >= 0; i--) {
        int j = num >> i & 1, k = limit >> i & 1;
        if (k) {
            if (son[p][j]) res += cnt[son[p][j]];
            if (!son[p][1 - j]) return res;
            p = son[p][1 - j];
        } else {
            if (!son[p][j]) return res;
            p = son[p][j];
        }
    }
    res += cnt[p];
    return res;
}

class Solution {
public:
    int countPairs(vector<int> &nums, int low, int high) {
        memset(son, 0, sizeof son), memset(cnt, 0, sizeof cnt);
        int res = 0;
        idx = 0;
        for (int x: nums) {
            res += query(x, high) - query(x, low - 1);
            insert(x);
        }
        return res;
    }
};
```