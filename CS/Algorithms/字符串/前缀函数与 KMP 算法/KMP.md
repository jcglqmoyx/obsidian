## [AcWing 831](https://www.acwing.com/problem/content/833/)          [LeetCode 28](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)        [Luogu 3375](https://www.luogu.com.cn/problem/P3375)

给定一个字符串 $S$，以及一个模式串 $P$，所有字符串中只包含大小写英文字母以及阿拉伯数字。

模式串 $P$ 在字符串 $S$  中多次作为子串出现。

求出模式串 $P$ 在字符串 $S$ 中所有出现的位置的起始下标。

### **输入格式**

第一行输入整数 $N$，表示字符串 $P$ 的长度。

第二行输入字符串 $P$。

第三行输入整数 $M$，表示字符串 $S$ 的长度。

第四行输入字符串 $S$。

### **输出格式**

共一行，输出所有出现位置的起始下标（下标从 $0$ 开始计数），整数之间用空格隔开。

### **数据范围**

$1$ ≤ $N$ ≤ $10^5$

1 ≤ $M$ ≤ $10^6$

### **输入样例：**

```
3
aba
5
ababa
```

### **输出样例：**

```
0 2
```

**下标从1开始的写法**

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 100010, M = 1000010;

int n, m;
int ne[N];
char s[M], p[N];

int main() {
    scanf("%d%s%d%s", &n, p + 1, &m, s + 1);

    for (int i = 2, j = 0; i <= n; i++) {
        while (j && p[i] != p[j + 1]) j = ne[j];
        if (p[i] == p[j + 1]) j++;
        ne[i] = j;
    }

    for (int i = 1, j = 0; i <= m; i++) {
        while (j && s[i] != p[j + 1]) j = ne[j];
        if (s[i] == p[j + 1]) j++;
        if (j == n) {
            printf("%d ", i - n);
            j = ne[j];
        }
    }
    return 0;
}
```

**下标从0开始的写法（不推荐）**

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1000010;

int n, m;
char s[N], p[N];
int ne[N];

int main() {
    scanf("%d%s%d%s", &n, p, &m, s);

    ne[0] = -1;
    for (int i = 1, j = -1; i < n; i++) {
        while (j >= 0 && p[i] != p[j + 1]) j = ne[j];
        if (p[i] == p[j + 1]) j++;
        ne[i] = j;
    }

    for (int i = 0, j = -1; i < m; i++) {
        while (j != -1 && s[i] != p[j + 1]) j = ne[j];
        if (s[i] == p[j + 1]) j++;
        if (j == n - 1) {
            cout << i - j << ' ';
            j = ne[j];
        }
    }
    return 0;
}
```

---

### **当子串为 “$ababc$” 时，ne数组值**

### 下标从 $1$ 开始

```cpp
ne[1] = 0
ne[2] = 0
ne[3] = 1
ne[4] = 2
ne[5] = 0
```

### 下标从 $0$ 开始

```cpp
ne[0] = -1
ne[1] = -1
ne[2] = 0
ne[3] = 1
ne[4] = -1
```