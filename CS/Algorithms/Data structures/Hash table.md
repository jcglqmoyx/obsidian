## [AcWing 840](https://www.acwing.com/problem/content/description/842/) (Template, without remove() operation)

维护一个集合，支持如下几种操作：

1. `I x`，插入一个数 x；
2. `Q x`，询问数 x 是否在集合中出现过；

现在要进行 $N$ 次操作，对于每个询问操作输出对应的结果。

### **输入格式**

第一行包含整数 $N$，表示操作数量。

接下来 $N$ 行，每行包含一个操作指令，操作指令为 `I x`，`Q x` 中的一种。

### **输出格式**

对于每个询问指令 `Q x`，输出一个询问结果，如果 x 在集合中出现过，则输出 `Yes`，否则输出 `No`。

每个结果占一行。

### **数据范围**

1 ≤ N ≤ $10^5$

$-10^9$ ≤ N ≤ $10^9$

### **输入样例：**

```
5
I 1
I 2
I 3
Q 2
Q 5
```

### **输出样例：**

```
Yes
No
```

### Open addressing

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 200003, null = 0x3f3f3f3f;

int h[N];

int find(int x) {
    int t = (x % N + N) % N;
    while (h[t] != null && h[t] != x) {
        t++;
        if (t == N) t = 0;
    }
    return t;
}

int main() {
    memset(h, 0x3f, sizeof h);

    int n;
    scanf("%d", &n);

    while (n--) {
        char op[2];
        int x;
        scanf("%s%d", op, &x);
        if (*op == 'I') h[find(x)] = x;
        else {
            if (h[find(x)] == null) puts("No");
            else puts("Yes");
        }
    }

    return 0;
}
```

### ****Separate chaining****

```
#include <bits/stdc++.h>

using namespace std;

const int N = 100003;

int h[N], e[N], ne[N], idx;

void insert(int x) {
    int k = (x % N + N) % N;
    e[idx] = x, ne[idx] = h[k], h[k] = idx++;
}

bool find(int x) {
    int k = (x % N + N) % N;
    for (int i = h[k]; i != -1; i = ne[i]) {
        if (e[i] == x) return true;
    }

    return false;
}

int main() {
    int n;
    scanf("%d", &n);

    memset(h, -1, sizeof h);

    while (n--) {
        char op[2];
        int x;
        scanf("%s%d", op, &x);

        if (*op == 'I') insert(x);
        else {
            if (find(x)) puts("Yes");
            else puts("No");
        }
    }

    return 0;
}
```

## [Luogu T124433](https://www.luogu.com.cn/problem/T124433) (More detailed template, with remove() operation)

### Separate chaining

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1000003;

int h[N], e[N], ne[N], idx;

void insert(int x) {
    int k = (x % N + N) % N;
    e[idx] = x, ne[idx] = h[k], h[k] = idx++;
}

bool find(int x) {
    int k = (x % N + N) % N;
    for (int i = h[k]; i != -1; i = ne[i]) {
        if (e[i] == x) return true;
    }
    return false;
}

bool remove(int x) {
    int k = (x % N + N) % N;
    if (e[h[k]] == x) {
        h[k] = ne[h[k]];
        return true;
    }
    for (int i = h[k]; i != -1; i = ne[i]) {
        if (e[ne[i]] == x) {
            ne[i] = ne[ne[i]];
            return true;
        }
    }
    return false;
}

int main() {
    int n;
    scanf("%d", &n);

    memset(h, -1, sizeof h);

    while (n--) {
        char op[10];
        int x;
        scanf("%s%d", op, &x);

        if (strcmp(op, "add") == 0) insert(x);
        else if (strcmp(op, "delete") == 0) {
            if (!remove(x)) puts("Failed");
        } else {
            if (find(x)) puts("True");
            else puts("False");
        }
    }
    return 0;
}
```

## [String hashing (Rabin–Karp algorithm)](https://www.acwing.com/problem/content/843/)

### [AcWing 841](https://www.acwing.com/problem/content/843/)

给定一个长度为 $n$ 的字符串，再给定 $m$ 个询问，每个询问包含四个整数 $l_1,r_1,l_2,r_2$，请你判断 $[l_1, r_1]$ 和 $[l_2, r_2]$ 这两个区间所包含的字符串子串是否完全相同。

字符串中只包含大小写英文字母和数字。

### **输入格式**

第一行包含整数 $n$ 和 $m$，表示字符串长度和询问次数。

第二行包含一个长度为 nn 的字符串，字符串中只包含大小写英文字母和数字。

接下来 $m$ 行，每行包含四个整数 $l_1,r_1,l_2,r_2$，表示一次询问所涉及的两个区间。

注意，字符串的位置从 $1$ 开始编号。

### **输出格式**

对于每个询问输出一个结果，如果两个字符串子串完全相同则输出 `Yes`，否则输出 `No`。

每个结果占一行。

### **数据范围**

1 ≤ n, m ≤ $10^5$

### **输入样例：**

```
8 3
aabbaabb
1 3 5 7
1 3 6 8
1 2 1 2
```

### **输出样例：**

```
Yes
No
Yes
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 100010, P = 13331;

typedef unsigned long long ULL;

ULL h[N], p[N];
char s[N];

ULL get(int l, int r) {
    return h[r] - h[l - 1] * p[r - l + 1];
}

int main() {
    int n, m;
    scanf("%d%d", &n, &m);
    scanf("%s", s + 1);

    p[0] = 1;
    for (int i = 1; i <= n; i++) {
        h[i] = h[i - 1] * P + s[i];
        p[i] = p[i - 1] * P;
    }

    while (m--) {
        int l1, r1, l2, r2;
        scanf("%d%d%d%d", &l1, &r1, &l2, &r2);
        cout << (get(l1, r1) == get(l2, r2) ? "Yes" : "No") << endl;
    }
    return 0;
}
```