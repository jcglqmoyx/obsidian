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

const int N = 2e7 + 10;

int n;
char a[N], b[N];
int p[N];

void init() {
    int k = 0;
    n = (int) strlen(a);
    b[k++] = '^', b[k++] = '#';
    for (int i = 0; i < n; i++) b[k++] = a[i], b[k++] = '#';
    b[k++] = '$';
    n = k;
}

void manacher() {
    int mr = 0, mid;
    for (int i = 1; i < n; i++) {
        if (i < mr) p[i] = min(p[(mid << 1) - i], mr - i);
        else p[i] = 1;
        while (b[i - p[i]] == b[i + p[i]]) p[i]++;
        if (i + p[i] > mr) {
            mr = i + p[i];
            mid = i;
        }
    }
}

int main() {
    scanf("%s", a);
    init();
    manacher();
    printf("%d", *max_element(p, p + n) - 1);
    return 0;
}
```