# Gray Code

## [LeetCode **89. Gray Code**](https://leetcode.cn/problems/gray-code/description/)

An **n-bit gray code sequence** is a sequence of$`2^n`$ integers where:

- Every integer is in the **inclusive** range `[0, $2^n$ - 1]`,
- The first integer is `0`,
- An integer appears **no more than once** in the sequence,
- The binary representation of every pair of **adjacent** integers differs by **exactly one bit**, and
- The binary representation of the **first** and **last** integers differs by **exactly one bit**.

Given an integer `n`, return *any valid **n-bit gray code sequence***.

**Example 1:**

```
Input: n = 2
Output: [0,1,3,2]
Explanation:
The binary representation of [0,1,3,2] is [00,01,11,10].
- 00 and 01 differ by one bit
-01 and11 differ by one bit
- 11 and 10 differ by one bit
-10 and00 differ by one bit
[0,2,3,1] is also a valid gray code sequence, whose binary representation is [00,10,11,01].
-00 and10 differ by one bit
- 10 and 11 differ by one bit
-11 and01 differ by one bit
- 01 and 00 differ by one bit
```

**Example 2:**

```
Input: n = 1
Output: [0,1]
```

**Constraints:**

- `1 <= n <= 16`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    vector<int> grayCode(int n) {
        vector<int> nums(1 << n);
        for (int i = 0; i < 1 << n; i++) {
            nums[i] = (i >> 1) ^ i;
        }
        return nums;
    }
};
```

## [LeetCode **1238. Circular Permutation in Binary Representation**](https://leetcode.cn/problems/circular-permutation-in-binary-representation/description/)

Given 2 integers `n` and `start`. Your task is return **any** permutation `p` of `(0,1,2.....,$2^n-1$)` such that :

- `p[0] = start`
- `p[i]` and `p[i+1]` differ by only one bit in their binary representation.
- `p[0]` and `p[$2^n$-1]` must also differ by only one bit in their binary representation.

**Example 1:**

```
Input: n = 2, start = 3
Output: [3,2,0,1]
Explanation: The binary representation of the permutation is (11,10,00,01).
All the adjacent element differ by one bit. Another valid permutation is [3,1,0,2]
```

**Example 2:**

```
Input: n = 3, start = 2
Output: [2,6,7,5,4,0,1,3]
Explanation: The binary representation of the permutation is (010,110,111,101,100,000,001,011).
```

**Constraints:**

- `1 <= n <= 16`
- `0 <= start < $2^n$`

```cpp
#include <bits/stdc++.h>

using namespace std;

class Solution {
public:
    vector<int> circularPermutation(int n, int start) {
        int m = 1 << n;
        vector<int> res(m);
        for (int i = 0; i < m; i++) {
            res[i] = start ^ i ^ (i >> 1);
        }
        return res;
    }
};
```