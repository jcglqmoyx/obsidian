---
page-title: 386. Lexicographical Numbers
url: https://leetcode.cn/problems/lexicographical-numbers/description/?envType=daily-question&envId=2024-09-21
date: 2024-09-21 15:13:08
---
Given an integer `n`, return all the numbers in the range `[1, n]` sorted in lexicographical order.

You must write an algorithm that runs in `O(n)` time and uses `O(1)` extra space. 

**Example 1:**

**Input:** n = 13
**Output:** \[1,10,11,12,13,2,3,4,5,6,7,8,9\]

**Example 2:**

**Input:** n = 2
**Output:** \[1,2\]

**Constraints:**

-   `1 <= n <= 5 * 104`

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    vector<int> lexicalOrder(int n) {  
        vector<int> res(n);  
        int num = 1;  
        for (int i = 0; i < n; i++) {  
            res[i] = num;  
            if (num * 10 <= n) num *= 10;  
            else {  
                while (num % 10 == 9 || num + 1 > n) num /= 10;  
                num++;  
            }  
        }  
        return res;  
    }  
};
```