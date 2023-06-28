# Trie

## [AcWing 835](https://www.acwing.com/problem/content/description/837/) （模板）

维护一个字符串集合，支持两种操作：

1. `I x` 向集合中插入一个字符串 $x$；
2. `Q x` 询问一个字符串在集合中出现了多少次。

共有 $N$ 个操作，所有输入的字符串总长度不超过 $10^5$，字符串仅包含小写英文字母。

### **输入格式**

第一行包含整数 $N$，表示操作数。

接下来 $N$ 行，每行包含一个操作指令，指令为 `I x` 或 `Q x` 中的一种。

### **输出格式**

对于每个询问指令 `Q x`，都要输出一个整数作为结果，表示 xx 在集合中出现的次数。

每个结果占一行。

### **数据范围**

1 ≤ $N$ ≤ $2 * 10^4$

### **输入样例：**

```
5
I abc
Q abc
Q ab
I ab
Q ab
```

### **输出样例：**

```
1
0
1
```

### 用结构体

```cpp
#include <bits/stdc++.h>

using namespace std;

struct Trie {
    int cnt = 0;
    Trie *ne[26];

    Trie() : cnt(0), ne() {}

    void insert(string &key) {
        Trie *node = this;
        for (char c: key) {
            if (!node->ne[c - 'a']) {
                node->ne[c - 'a'] = new Trie();
            }
            node = node->ne[c - 'a'];
        }
        node->cnt++;
    }

    int query(string &key) {
        Trie *node = this;
        for (char c: key) {
            if (!node->ne[c - 'a']) {
                return 0;
            }
            node = node->ne[c - 'a'];
        }
        return node->cnt;
    }
};

int main() {
    int n;
    cin >> n;
    Trie *trie = new Trie();
    while (n--) {
        char op;
        string key;
        cin >> op >> key;
        if (op == 'I') {
            trie->insert(key);
        } else {
            cout << trie->query(key) << endl;
        }
    }
    return 0;
}
```

### 用数组模拟

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 100010;

int son[N][26], cnt[N], p, idx;
char str[N];

void insert(const char s[]) {
    p = 0;
    for (int i = 0; s[i]; i++) {
        int u = s[i] - 'a';
        if (!son[p][u]) son[p][u] = ++idx;
        p = son[p][u];
    }
    cnt[p]++;
}

int query(const char s[]) {
    p = 0;
    for (int i = 0; s[i]; i++) {
        int u = s[i] - 'a';
        if (!son[p][u]) return 0;
        p = son[p][u];
    }
    return cnt[p];
}

int main() {
    int n;
    scanf("%d", &n);
    while (n--) {
        char op[2];
        scanf("%s%s", op, str);
        if (*op == 'I') {
            insert(str);
        } else {
            printf("%d\n", query(str));
        }
    }
    return 0;
}
```

## [AcWing 143. 最大异或对](https://www.acwing.com/problem/content/description/145/)

在给定的 $N$ 个整数 $A_1$，$A_2$ … $A_n$ 中选出两个进行 $xor$（异或）运算，得到的结果最大是多少？

### **输入格式**

第一行输入一个整数 $N$。

第二行输入 $N$ 个整数 $A_1$~$A_n$。

### **输出格式**

输出一个整数表示答案。

### **数据范围**

$1$ ≤ $N$ ≤ $10^5$

$0$ ≤ $A_i$ ≤ $2^{31}$

### **输入样例：**

```
3
1 2 3
```

### **输出样例：**

```
3
```

### 用结构体：

```cpp
#include <bits/stdc++.h>

using namespace std;

struct Trie {
    Trie *ne[2];

    Trie() : ne() {}

    void insert(int x) {
        auto trie = this;
        for (int i = 30; ~i; i--) {
            int u = x >> i & 1;
            if (!trie->ne[u]) trie->ne[u] = new Trie();
            trie = trie->ne[u];
        }
    }

    int query(int x) {
        auto trie = this;
        int res = 0;
        for (int i = 30; ~i; i--) {
            if (!trie) break;
            int u = x >> i & 1;
            if (trie->ne[u ^ 1]) {
                trie = trie->ne[u ^ 1];
                res += 1 << i;
            } else {
                trie = trie->ne[u];
            }
        }
        return res;
    }
};

int main() {
    auto trie = new Trie();
    int n;
    scanf("%d", &n);
    int res = 0;
    for (int i = 0; i < n; i++) {
        int x;
        scanf("%d", &x);
        res = max(res, trie->query(x));
        trie->insert(x);
    }
    printf("%d\n", res);
    return 0;
}
```

### 用数组模板Trie：

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1e5 + 10;

int n;
int son[N * 31][2], idx;

void insert(int x) {
    int p = 0;
    for (int i = 30; i >= 0; i--) {
        int u = x >> i & 1;
        if (!son[p][u]) son[p][u] = ++idx;
        p = son[p][u];
    }
}

int query(int x) {
    int res = 0, p = 0;
    for (int i = 30; i >= 0; i--) {
        int u = x >> i & 1;
        if (!son[p][u ^ 1]) {
            if (!son[p][u]) break;
            p = son[p][u];
        } else {
            p = son[p][u ^ 1];
            res += 1 << i;
        }
    }
    return res;
}

int main() {
    scanf("%d", &n);
    int res = 0;
    while (n--) {
        int x;
        scanf("%d", &x);
        res = max(res, query(x));
        insert(x);
    }
    printf("%d\n", res);
    return 0;
}
```

## [LeetCode 1707. Maximum XOR With an Element From Array](https://leetcode.cn/problems/maximum-xor-with-an-element-from-array/)

You are given an array `nums` consisting of non-negative integers. You are also given a `queries` array, where `queries[i] = [$x_i$, $m_i$]`.

The answer to the `ith` query is the maximum bitwise `XOR` value of $x_i$ and any element of `nums` that does not exceed $m_i$. In other words, the answer is `max(nums[j] XOR $x_i$)` for all `j` such that `nums[j] <= $m_i$`. If all elements in `nums` are larger than $m_i$, then the answer is `-1`.

Return *an integer array* `answer` *where* `answer.length == queries.length` *and* `answer[i]` *is the answer to the $i^{th}$ query.*

**Example 1:**

```
**Input**: nums = [0,1,2,3,4], queries = [[3,1],[1,3],[5,6]]
**Output**: [3,3,7]
**Explanation**:
1) 0 and 1 are the only two integers not greater than 1. 0 XOR 3 = 3 and 1 XOR 3 = 2. The larger of the two is 3.

2) 1 XOR 2 = 3.

3) 5 XOR 2 = 7.
```

**Example 2:**

```
**Input**: nums = [5,2,4,6,6,3], queries = [[12,4],[8,1],[6,3]]
**Output**: [15,-1,5]
```

**Constraints:**

- `1 <= nums.length, queries.length <= $10^5$`
- `queries[i].length == 2`
- `0 <= nums[j], xi, mi <= $10^9$`

### Using struct and offline query

```cpp
#include <bits/stdc++.h>

using namespace std;

struct Tree {
    Tree *ne[2];

    Tree() : ne() {}

    void insert(int num) {
        Tree *t = this;
        for (int i = 30; ~i; i--) {
            int bit = num >> i & 1;
            if (!t->ne[bit]) t->ne[bit] = new Tree();
            t = t->ne[bit];
        }
    }
};

class Solution {
public:
    vector<int> maximizeXor(vector<int> &nums, vector<vector<int>> &queries) {
        sort(nums.begin(), nums.end());
        int n = (int) queries.size();
        for (int i = 0; i < n; i++) {
            queries[i].push_back(i);
        }
        sort(queries.begin(), queries.end(), [](auto &a, auto &b) {
            return a[1] < b[1];
        });
        vector<int> res(n);
        res.reserve(queries.size());
        auto *tree = new Tree();
        int p = 0;
        for (auto &q: queries) {
            int x = q[0], m = q[1], idx = q[2];
            if (nums.front() > m) {
                res[idx] = -1;
                continue;
            }
            while (p < nums.size() && nums[p] <= m) {
                tree->insert(nums[p]);
                p++;
            }
            auto t = tree;
            int sum = 0;
            for (int i = 30; ~i; i--) {
                int bit = (x >> i) & 1;
                int j = bit ^ 1;
                if (t->ne[j]) {
                    t = t->ne[j];
                    sum += 1 << i;
                } else if (t->ne[bit]) {
                    t = t->ne[bit];
                } else {
                    break;
                }
            }
            res[idx] = sum;
        }
        return res;
    }
};
```

### Using array-based Trie and online query

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 100010, M = N * 31;

int son[M][2], idx;
int mn[M];

void insert(int x) {
    int p = 0;
    for (int i = 30; i >= 0; i--) {
        int u = x >> i & 1;
        if (!son[p][u]) son[p][u] = ++idx;
        p = son[p][u];
        mn[p] = min(mn[p], x);
    }
}

int query(int num, int limit) {
    int res = 0, p = 0;
    for (int i = 30; i >= 0; i--) {
        int j = num >> i & 1;
        if (son[p][j ^ 1]) {
            if (mn[son[p][j ^ 1]] <= limit) {
                res += 1 << i;
                p = son[p][j ^ 1];
            } else {
                if (!son[p][j]) return -1;
                p = son[p][j];
            }
        } else {
            if (son[p][j] && mn[son[p][j]] <= limit) {
                p = son[p][j];
            }
        }
    }
    return res;
}

class Solution {
public:
    vector<int> maximizeXor(vector<int> &nums, vector<vector<int>> &queries) {
        memset(son, 0, sizeof son), memset(mn, 0x3f, sizeof mn), idx = 0;
        for (int x: nums) insert(x);
        int n = (int) queries.size();
        vector<int> res(n);
        for (int i = 0; i < n; i++) {
            auto &q = queries[i];
            int x = q[0], m = q[1];
            res[i] = query(x, m);
        }
        return res;
    }
};
```

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