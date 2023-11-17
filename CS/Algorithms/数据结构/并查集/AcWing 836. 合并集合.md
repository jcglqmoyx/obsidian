## [AcWing 836 (模板）](https://www.acwing.com/problem/content/838/)

一共有 $n$ 个数，编号是 $1$ ~ $n$，最开始每个数各自在一个集合中。

现在要进行 m 个操作，操作共有两种：

1. `M a b`，将编号为 $a$ 和 $b$ 的两个数所在的集合合并，如果两个数已经在同一个集合中，则忽略这个操作；
2. `Q a b`，询问编号为  和  的两个数是否在同一个集合中；

### **输入格式**

第一行输入整数 $n$ 和 $m$。

接下来 $m$ 行，每行包含一个操作指令，指令为 `M a b` 或 `Q a b` 中的一种。

### **输出格式**

对于每个询问指令 `Q a b`，都要输出一个结果，如果 $a$ 和 $b$ 在同一集合内，则输出 `Yes`，否则输出 `No`。

每个结果占一行。

### **数据范围**

$1$ ≤ $n$, $m$ ≤ $10^5$

### **输入样例：**

```
4 5
M 1 2
M 3 4
Q 1 2
Q 1 3
Q 3 4
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

const int N = 100010;

int n, m;
int p[N], sz[N];

int find(int x) {
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

void merge(int x, int y) {
    int px = find(x), py = find(y);
    if (px == py) return;
    if (sz[px] > sz[py]) swap(px, py);
    p[px] = py;
    sz[py] += sz[px];
}

int main() {
    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; i++) p[i] = i;
    char op[2];
    int x, y;
    while (m--) {
        scanf("%s%d%d", op, &x, &y);
        if (op[0] == 'M') merge(x, y);
        else if (op[0] == 'Q') {
            if (find(x) == find(y)) puts("Yes");
            else puts("No");
        }
    }
    return 0;
}
```