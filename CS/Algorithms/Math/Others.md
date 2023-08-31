## [LeetCode 1015. Smallest Integer Divisible by K](https://leetcode.cn/problems/smallest-integer-divisible-by-k/)
Given a positive integer `k`, you need to find the **length** of the **smallest** positive integer `n` such that `n` is divisible by `k`, and `n` only contains the digit `1`.

Return *the **length** of* `n`. If there is no such `n`, return -1.

**Note:** `n` may not fit in a 64-bit signed integer.

**Example 1:**

**Input:** k = 1
**Output:** 1
**Explanation:** The smallest answer is n = 1, which has length 1.

**Example 2:**

**Input:** k = 2
**Output:** -1
**Explanation:** There is no such positive integer n divisible by 2.

**Example 3:**

**Input:** k = 3
**Output:** 3
**Explanation:** The smallest answer is n = 111, which has length 3.

**Constraints:**

-   `1 <= k <= 100000`
```cpp
class Solution {
public:
    int smallestRepunitDivByK(int k) {
        if (k % 2 == 0 || k % 5 == 0) return -1;
        int x = 1 % k, len = 1;
        while (x) x = (x * 10 + 1) % k, len++;
        return len;
    }
};
```
## [LeetCode 1016. Binary String With Substrings Representing 1 To N](https://leetcode.cn/problems/binary-string-with-substrings-representing-1-to-n/)
---
Given a binary string `s` and a positive integer `n`, return `true` *if the binary representation of all the integers in the range* `[1, n]` *are **substrings** of* `s`*, or* `false` *otherwise*.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** s = "0110", n = 3
**Output:** true

**Example 2:**

**Input:** s = "0110", n = 4
**Output:** false

**Constraints:**

-   `1 <= s.length <= 1000`
-   `s[i]` is either `'0'` or `'1'`.
-   `1 <= n <= 1e9`
```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    bool help(const string &s, int k, int mi, int ma) {
        unordered_set<int> st;
        int t = 0;
        for (int r = 0; r < s.size(); ++r) {
            t = t * 2 + (s[r] - '0');
            if (r >= k) t -= (s[r - k] - '0') << k;
            if (r >= k - 1 && t >= mi && t <= ma) st.insert(t);
        }
        return st.size() == ma - mi + 1;
    }

    bool queryString(string s, int n) {
        if (n == 1) return s.find('1') != -1;
        int k = 30;
        while ((1 << k) >= n) k--;
        if (s.size() < (1 << (k - 1)) + k - 1 || s.size() < n - (1 << k) + k + 1) return false;
        return help(s, k, 1 << (k - 1), (1 << k) - 1) && help(s, k + 1, 1 << k, n);
    }
};
```
## [LeetCode 2335. Minimum Amount of Time to Fill Cups](https://leetcode.cn/problems/minimum-amount-of-time-to-fill-cups/)

You have a water dispenser that can dispense cold, warm, and hot water. Every second, you can either fill up `2` cups with **different** types of water, or `1` cup of any type of water.

You are given a **0-indexed** integer array `amount` of length `3` where `amount[0]`, `amount[1]`, and `amount[2]` denote the number of cold, warm, and hot water cups you need to fill respectively. Return *the **minimum** number of seconds needed to fill up all the cups*.

**Example 1:**

```
Input: amount = [1,4,2]
Output: 4
Explanation: One way to fill up the cups is:
Second 1: Fill up a cold cup and a warm cup.
Second 2: Fill up a warm cup and a hot cup.
Second 3: Fill up a warm cup and a hot cup.
Second 4: Fill up a warm cup.
It can be proven that 4 is the minimum number of seconds needed.
```

**Example 2:**

```
Input: amount = [5,4,4]
Output: 7
Explanation: One way to fill up the cups is:
Second 1: Fill up a cold cup, and a hot cup.
Second 2: Fill up a cold cup, and a warm cup.
Second 3: Fill up a cold cup, and a warm cup.
Second 4: Fill up a warm cup, and a hot cup.
Second 5: Fill up a cold cup, and a hot cup.
Second 6: Fill up a cold cup, and a warm cup.
Second 7: Fill up a hot cup.
```

**Example 3:**

```
Input: amount = [5,0,0]
Output: 5
Explanation: Every second, we fill up a cold cup.
```

**Constraints:**

- `amount.length == 3`
- `0 <= amount[i] <= 100`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    int fillCups(vector<int> &a) {
        return max({a[0], a[1], a[2], (a[0] + a[1] + a[2] + 1) / 2});
    }
};
```
