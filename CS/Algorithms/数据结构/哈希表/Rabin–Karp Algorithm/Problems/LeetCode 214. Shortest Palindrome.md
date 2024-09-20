---
page-title: 214. Shortest Palindrome
url: https://leetcode.com/problems/shortest-palindrome/description/?envType=daily-question&envId=2024-09-20
date: 2024-09-20 09:55:34
---
You are given a string `s`. You can convert `s` to a palindrome by adding characters in front of it.

Return *the shortest palindrome you can find by performing this transformation*.

**Example 1:**

**Input:** s = "aacecaaa"
**Output:** "aaacecaaa"

**Example 2:**

**Input:** s = "abcd"
**Output:** "dcbabcd"

**Constraints:**

-   `0 <= s.length <= 5 * 104`
-   `s` consists of lowercase English letters only.

单哈希
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    string shortestPalindrome(string s) {  
        using LL = long long;  
        const LL BASE = 131, MOD = 1e9 + 7;  
        auto n = s.size();  
        int len = 0;  
        LL l = 0, r = 0, mul = 1;  
        for (int i = 0; i < n; i++) {  
            l = (l * BASE + s[i]) % MOD;  
            r = (s[i] * mul + r) % MOD;  
            mul = mul * BASE % MOD;  
            if (l == r) len = i + 1;  
        }  
        auto t = s.substr(len);  
        reverse(t.begin(), t.end());  
        return t + s;  
    }  
};
```

双哈希
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    string shortestPalindrome(string s) {  
        using LL = long long;  
        const LL BASE1 = 131, MOD1 = 1e9 + 7;  
        const LL BASE2 = 13331, MOD2 = 1e9 + 9;  
        auto n = s.size();  
        int len = 0;  
        pair<int, int> l, r;  
        LL mul1 = 1, mul2 = 1;  
        for (int i = 0; i < n; i++) {  
            l = {(l.first * BASE1 + s[i]) % MOD1, (l.second * BASE2 + s[i]) % MOD2};  
            r = {(s[i] * mul1 + r.first) % MOD1, (s[i] * mul2 + r.second) % MOD2};  
            mul1 = mul1 * BASE1 % MOD1;  
            mul2 = mul2 * BASE2 % MOD2;  
            if (l == r) len = i + 1;  
        }  
        auto t = s.substr(len);  
        reverse(t.begin(), t.end());  
        return t + s;  
    }  
};
```

KMP
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    string shortestPalindrome(string s) {  
        string t = s;  
        reverse(t.begin(), t.end());  
        t = ' ' + s + '#' + t;  
        auto n = t.size();  
        int ne[n];  
        ne[1] = 0;  
        for (int i = 2, j = 0; i < n; i++) {  
            while (j && t[i] != t[j + 1]) j = ne[j];  
            if (t[i] == t[j + 1]) j++;  
            ne[i] = j;  
        }  
        int len = ne[n - 1];  
        t = s.substr(len);  
        reverse(t.begin(), t.end());  
        return t + s;  
    }  
};
```