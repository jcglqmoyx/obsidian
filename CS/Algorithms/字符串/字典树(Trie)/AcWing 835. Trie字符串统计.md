## [AcWing 835](https://www.acwing.com/problem/content/description/837/) （模板）

维护一个字符串集合，支持两种操作：

1. `I x` 向集合中插入一个字符串 $x$；
2. `Q x` 询问一个字符串在集合中出现了多少次。

共有 $N$ 个操作，所有输入的字符串总长度不超过 $10^5$，字符串仅包含小写英文字母。

### **输入格式**

第一行包含整数 $N$，表示操作数。

接下来 $N$ 行，每行包含一个操作指令，指令为 `I x` 或 `Q x` 中的一种。

### **输出格式**

对于每个询问指令 `Q x`，都要输出一个整数作为结果，表示 xx 在集合中出现的次数。

每个结果占一行。

### **数据范围**

1 ≤ $N$ ≤ $2 * 10^4$

### **输入样例：**

```
5
I abc
Q abc
Q ab
I ab
Q ab
```

### **输出样例：**

```
1
0
1
```

### 用结构体

```cpp
#include <bits/stdc++.h>

using namespace std;

struct Trie {
    int cnt = 0;
    Trie *ne[26];

    Trie() : cnt(0), ne() {}

    void insert(string &key) {
        Trie *node = this;
        for (char c: key) {
            if (!node->ne[c - 'a']) {
                node->ne[c - 'a'] = new Trie();
            }
            node = node->ne[c - 'a'];
        }
        node->cnt++;
    }

    int query(string &key) {
        Trie *node = this;
        for (char c: key) {
            if (!node->ne[c - 'a']) {
                return 0;
            }
            node = node->ne[c - 'a'];
        }
        return node->cnt;
    }
};

int main() {
    int n;
    cin >> n;
    Trie *trie = new Trie();
    while (n--) {
        char op;
        string key;
        cin >> op >> key;
        if (op == 'I') {
            trie->insert(key);
        } else {
            cout << trie->query(key) << endl;
        }
    }
    return 0;
}
```

### 用数组模拟

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 100010;

int son[N][26], cnt[N], p, idx;
char str[N];

void insert(const char s[]) {
    p = 0;
    for (int i = 0; s[i]; i++) {
        int u = s[i] - 'a';
        if (!son[p][u]) son[p][u] = ++idx;
        p = son[p][u];
    }
    cnt[p]++;
}

int query(const char s[]) {
    p = 0;
    for (int i = 0; s[i]; i++) {
        int u = s[i] - 'a';
        if (!son[p][u]) return 0;
        p = son[p][u];
    }
    return cnt[p];
}

int main() {
    int n;
    scanf("%d", &n);
    while (n--) {
        char op[2];
        scanf("%s%s", op, str);
        if (*op == 'I') {
            insert(str);
        } else {
            printf("%d\n", query(str));
        }
    }
    return 0;
}
```