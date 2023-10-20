---
page-title: "341. 扁平化嵌套列表迭代器 - 力扣（LeetCode）"
url: https://leetcode.cn/problems/flatten-nested-list-iterator/description/?envType=daily-question&envId=2023-10-20
date: "2023-10-20 08:26:30"
---
You are given a nested list of integers `nestedList`. Each element is either an integer or a list whose elements may also be integers or other lists. Implement an iterator to flatten it.

Implement the `NestedIterator` class:

-   `NestedIterator(List<NestedInteger> nestedList)` Initializes the iterator with the nested list `nestedList`.
-   `int next()` Returns the next integer in the nested list.
-   `boolean hasNext()` Returns `true` if there are still some integers in the nested list and `false` otherwise.

Your code will be tested with the following pseudocode:

initialize iterator with nestedList
res = \[\]
while iterator.hasNext()
    append iterator.next() to the end of res
return res

If `res` matches the expected flattened list, then your code will be judged as correct.

**Example 1:**

**Input:** nestedList = \[\[1,1\],2,\[1,1\]\]
**Output:** \[1,1,2,1,1\]
**Explanation:** By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: \[1,1,2,1,1\].

**Example 2:**

**Input:** nestedList = \[1,\[4,\[6\]\]\]
**Output:** \[1,4,6\]
**Explanation:** By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: \[1,4,6\].

**Constraints:**

-   `1 <= nestedList.length <= 500`
-   The values of the integers in the nested list is in the range `[-106, 106]`.

```cpp
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */

class NestedIterator {
    queue<int> q;

    void dfs(vector <NestedInteger> nestedList) {
        for (auto t: nestedList) {
            if (t.isInteger()) {
                q.emplace(t.getInteger());
            } else {
                dfs(t.getList());
            }
        }
    }

public:
    NestedIterator(vector <NestedInteger> &nestedList) {
        dfs(nestedList);
    }

    int next() {
        int res = q.front();
        q.pop();
        return res;
    }

    bool hasNext() {
        return !q.empty();
    }
};
```

```cpp
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */

class NestedIterator {
private:
    stack<pair<vector<NestedInteger>::iterator, vector<NestedInteger>::iterator>> stk;

public:
    NestedIterator(vector<NestedInteger> &nestedList) {
        stk.emplace(nestedList.begin(), nestedList.end());
    }

    int next() {
        return stk.top().first++->getInteger();
    }

    bool hasNext() {
        while (!stk.empty()) {
            auto &p = stk.top();
            if (p.first == p.second) {
                stk.pop();
                continue;
            }
            if (p.first->isInteger()) {
                return true;
            }
            auto &lst = p.first++->getList();
            stk.emplace(lst.begin(), lst.end());
        }
        return false;
    }
};
```