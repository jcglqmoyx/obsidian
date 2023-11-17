## [AcWing 143. 最大异或对](https://www.acwing.com/problem/content/description/145/)

在给定的 $N$ 个整数 $A_1$，$A_2$ … $A_n$ 中选出两个进行 $xor$（异或）运算，得到的结果最大是多少？

### **输入格式**

第一行输入一个整数 $N$。

第二行输入 $N$ 个整数 $A_1$~$A_n$。

### **输出格式**

输出一个整数表示答案。

### **数据范围**

$1$ ≤ $N$ ≤ $10^5$

$0$ ≤ $A_i$ ≤ $2^{31}$

### **输入样例：**

```
3
1 2 3
```

### **输出样例：**

```
3
```

### 用结构体：

```cpp
#include <bits/stdc++.h>

using namespace std;

struct Trie {
    Trie *ne[2];

    Trie() : ne() {}

    void insert(int x) {
        auto trie = this;
        for (int i = 30; ~i; i--) {
            int u = x >> i & 1;
            if (!trie->ne[u]) trie->ne[u] = new Trie();
            trie = trie->ne[u];
        }
    }

    int query(int x) {
        auto trie = this;
        int res = 0;
        for (int i = 30; ~i; i--) {
            if (!trie) break;
            int u = x >> i & 1;
            if (trie->ne[u ^ 1]) {
                trie = trie->ne[u ^ 1];
                res += 1 << i;
            } else {
                trie = trie->ne[u];
            }
        }
        return res;
    }
};

int main() {
    auto trie = new Trie();
    int n;
    scanf("%d", &n);
    int res = 0;
    for (int i = 0; i < n; i++) {
        int x;
        scanf("%d", &x);
        res = max(res, trie->query(x));
        trie->insert(x);
    }
    printf("%d\n", res);
    return 0;
}
```

### 用数组模板Trie：

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1e5 + 10;

int n;
int son[N * 31][2], idx;

void insert(int x) {
    int p = 0;
    for (int i = 30; i >= 0; i--) {
        int u = x >> i & 1;
        if (!son[p][u]) son[p][u] = ++idx;
        p = son[p][u];
    }
}

int query(int x) {
    int res = 0, p = 0;
    for (int i = 30; i >= 0; i--) {
        int u = x >> i & 1;
        if (!son[p][u ^ 1]) {
            if (!son[p][u]) break;
            p = son[p][u];
        } else {
            p = son[p][u ^ 1];
            res += 1 << i;
        }
    }
    return res;
}

int main() {
    scanf("%d", &n);
    int res = 0;
    while (n--) {
        int x;
        scanf("%d", &x);
        res = max(res, query(x));
        insert(x);
    }
    printf("%d\n", res);
    return 0;
}
```