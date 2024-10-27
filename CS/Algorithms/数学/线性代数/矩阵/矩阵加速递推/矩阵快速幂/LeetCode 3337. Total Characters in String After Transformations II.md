---
page-title: LeetCode 3337. Total Characters in String After Transformations II
url: https://leetcode.com/problems/total-characters-in-string-after-transformations-ii/description/
date: 2024-10-27 18:56:28
---
You are given a string `s` consisting of lowercase English letters, an integer `t` representing the number of **transformations** to perform, and an array `nums` of size 26. In one **transformation**, every character in `s` is replaced according to the following rules:

- Replace `s[i]` with the **next** `nums[s[i] - 'a']` consecutive characters in the alphabet. For example, if `s[i] = 'a'` and `nums[0] = 3`, the character `'a'` transforms into the next 3 consecutive characters ahead of it, which results in `"bcd"`.
- The transformation **wraps** around the alphabet if it exceeds `'z'`. For example, if `s[i] = 'y'` and `nums[24] = 3`, the character `'y'` transforms into the next 3 consecutive characters ahead of it, which results in `"zab"`.

Return the length of the resulting string after **exactly** `t` transformations.

Since the answer may be very large, return it **modulo** `109 + 7`.

**Example 1:**

**Input:** s = "abcyy", t = 2, nums = [1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,2]

**Output:** 7

**Explanation:**

- **First Transformation (t = 1):**
    
    - `'a'` becomes `'b'` as `nums[0] == 1`
    - `'b'` becomes `'c'` as `nums[1] == 1`
    - `'c'` becomes `'d'` as `nums[2] == 1`
    - `'y'` becomes `'z'` as `nums[24] == 1`
    - `'y'` becomes `'z'` as `nums[24] == 1`
    - String after the first transformation: `"bcdzz"`
- **Second Transformation (t = 2):**
    
    - `'b'` becomes `'c'` as `nums[1] == 1`
    - `'c'` becomes `'d'` as `nums[2] == 1`
    - `'d'` becomes `'e'` as `nums[3] == 1`
    - `'z'` becomes `'ab'` as `nums[25] == 2`
    - `'z'` becomes `'ab'` as `nums[25] == 2`
    - String after the second transformation: `"cdeabab"`
- **Final Length of the string:** The string is `"cdeabab"`, which has 7 characters.
    

**Example 2:**

**Input:** s = "azbk", t = 1, nums = [2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2]

**Output:** 8

**Explanation:**

- **First Transformation (t = 1):**
    
    - `'a'` becomes `'bc'` as `nums[0] == 2`
    - `'z'` becomes `'ab'` as `nums[25] == 2`
    - `'b'` becomes `'cd'` as `nums[1] == 2`
    - `'k'` becomes `'lm'` as `nums[10] == 2`
    - String after the first transformation: `"bcabcdlm"`
- **Final Length of the string:** The string is `"bcabcdlm"`, which has 8 characters.

**Constraints:**

- `1 <= s.length <= 105`
- `s` consists only of lowercase English letters.
- `1 <= t <= 109`
- `nums.length == 26`
- `1 <= nums[i] <= 25`

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
using LL = long long;  
  
static constexpr int MOD = 1'000'000'007;  
  
vector<vector<LL>> multiply(vector<vector<LL>> a, vector<vector<LL>> b) {  
    auto rows = a.size(), columns = b[0].size(), temp = b.size();  
    vector<vector<LL>> c(rows, vector<LL>(columns));  
    for (int i = 0; i < rows; i++) {  
        for (int j = 0; j < columns; j++) {  
            for (int k = 0; k < temp; k++) {  
                c[i][j] = (c[i][j] + a[i][k] * b[k][j]) % MOD;  
            }  
        }  
    }  
    return c;  
}  
  
vector<vector<LL>> pow(vector<vector<LL>> mat, int n) {  
    vector<vector<LL>> ret = mat;  
    n--;  
    while (n > 0) {  
        if ((n & 1) == 1) {  
            ret = multiply(ret, mat);  
        }  
        n >>= 1;  
        mat = multiply(mat, mat);  
    }  
    return ret;  
}  
  
class Solution {  
public:  
    int lengthAfterTransformations(string s, int t, vector<int> &nums) {  
        vector<vector<LL>> cnt(26, vector<LL>(1));  
        for (char c: s) {  
            cnt[c - 'a'][0]++;  
        }  
  
        vector<vector<LL>> mat(26, vector<LL>(26));  
        for (int i = 0; i < 26; i++) {  
            for (int x = i + 1; x <= i + nums[i]; x++) {  
                mat[x % 26][i] = 1;  
            }  
        }  
        auto matrix = multiply(pow(mat, t), cnt);  
  
        LL res = 0;  
        for (int i = 0; i < 26; i++) {  
            res = (res + matrix[i][0]) % MOD;  
        }  
        return res;  
    }  
};
```