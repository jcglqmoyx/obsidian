---
page-title: 3188. manacher算法 - AcWing题库
url: https://www.acwing.com/problem/content/3190/
date: 2023-10-27 10:44:46
---
给定一个长度为 n 的由小写字母构成的字符串，求它的最长回文子串的长度是多少。

#### 输入格式

一个由小写字母构成的字符串。

#### 输出格式

输出一个整数，表示最长回文子串的长度。

#### 数据范围

1≤n≤107

#### 输入样例：

```
abcbabcbabcba
```

#### 输出样例：

```
13
```

```cpp
#pragma GCC optimize("Ofast", "inline", "-ffast-math")
#pragma GCC target("avx,sse2,sse3,sse4,mmx")

#include <bits/stdc++.h>  
  
using namespace std;  
  
const int N = 1e7 + 5;  
  
char s[N];  
char t[N << 1];  
int d[N << 1];  
  
void solve() {  
    scanf("%s", s);  
    t[0] = '^', t[1] = '#';  
    int n = 2;  
    auto len = strlen(s);  
    for (int i = 0; i < len; i++) {  
        t[n++] = s[i];  
        t[n++] = '#';  
    }  
    t[n++] = '$';  
  
    d[1] = 1;  
    int res = 0;  
    for (int l = 1, r = 1, i = 2; i < n; i++) {  
        if (i <= r) d[i] = min(d[l + r - i], r - i + 1);  
        while (t[i - d[i]] == t[i + d[i]]) d[i]++;  
        if (i + d[i] - 1 > r) l = i - d[i] + 1, r = i + d[i] - 1;  
        res = max(res, d[i] - 1);  
    }  
    printf("%d\n", res);  
}  
  
int main() {  
    solve();  
    return 0;  
}
```


[洛谷 P3805 【模板】manacher](https://www.luogu.com.cn/problem/P3805)