## [LeetCode 1053. Previous Permutation With One Swap](https://leetcode.cn/problems/previous-permutation-with-one-swap/)
Given an array of positive integers `arr` (not necessarily distinct), return _the_ lexicographically _largest permutation that is smaller than_ `arr`, that can be **made with exactly one swap**. If it cannot be done, then return the same array.

**Note** that a _swap_ exchanges the positions of two numbers `arr[i]` and `arr[j]`

**Example 1:**
```
Input: arr = [3,2,1]
Output: [3,1,2]
Explanation: Swapping 2 and 1.
```

**Example 2:**
```
Input: arr = [1,1,5]
Output: [1,1,5]
Explanation: This is already the smallest permutation.
```

**Example 3:**
```
Input: arr = [1,9,4,6,7]
Output: [1,7,4,6,9]
Explanation: Swapping 9 and 7.
```

**Constraints:**
1 <= arr.length <= $10^4$
1 <= arr[i] <= $10^4$
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    vector<int> prevPermOpt1(vector<int> &arr) {  
        int n = (int) arr.size();  
        for (int i = n - 2; i >= 0; i--) {  
            if (arr[i] > arr[i + 1]) {  
                int j = n - 1;  
                while (arr[j] >= arr[i] || arr[j] == arr[j - 1]) j--;  
                swap(arr[i], arr[j]);  
                break;            }  
        }  
        return arr;  
    }  
};
```