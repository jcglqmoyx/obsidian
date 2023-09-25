## [AcWing 836 (template）](https://www.acwing.com/problem/content/838/)

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

## [AcWing 240 (扩展应用）](https://www.acwing.com/problem/content/description/242/)

动物王国中有三类动物 $A,B,C$，这三类动物的食物链构成了有趣的环形。

$A$ 吃 $B$，$B$ 吃 $C$，$C$ 吃 $A$。

现有 $N$ 个动物，以 $1$~$N$ 编号。

每个动物都是 $A,B,C$ 中的一种，但是我们并不知道它到底是哪一种。

有人用两种说法对这 $N$ 个动物所构成的食物链关系进行描述：

第一种说法是 `1 X Y`，表示 $X$ 和 $Y$ 是同类。

第二种说法是 `2 X Y`，表示 $X$ 吃 $Y$。

此人对 $N$ 个动物，用上述两种说法，一句接一句地说出 $K$ 句话，这 $K$ 句话有的是真的，有的是假的。

当一句话满足下列三条之一时，这句话就是假话，否则就是真话。

1. 当前的话与前面的某些真的话冲突，就是假话；
2. 当前的话中 $X$ 或 $Y$ 比 $N$ 大，就是假话；
3. 当前的话表示 $X$ 吃 $X$，就是假话。

你的任务是根据给定的 $N$ 和 $K$ 句话，输出假话的总数。

### **输入格式**

第一行是两个整数 $N$ 和 $K$，以一个空格分隔。

以下 KK 行每行是三个正整数 $D,X,Y$，两数之间用一个空格隔开，其中 $D$ 表示说法的种类。

若 $D=1$，则表示 $X$ 和 $Y$ 是同类。

若 $D=2$，则表示 $X$ 吃 $Y$。

### **输出格式**

只有一个整数，表示假话的数目。

### **数据范围**

1 ≤ $N$ ≤ 50000,

0 ≤ $K$ ≤ 100000

### **输入样例：**

```
100 7
1 101 1
2 1 2
2 2 3
2 3 3
1 1 3
2 3 1
1 5 5
```

### **输出样例：**

```
3
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 50010;

int n, k;
int p[N], d[N];

int find(int x) {
    if (p[x] != x) {
        int t = find(p[x]);
        d[x] += d[p[x]];
        p[x] = t;
    }
    return p[x];
}

int main() {
    scanf("%d%d", &n, &k);
    for (int i = 1; i <= n; i++) p[i] = i;
    int res = 0;
    int op, x, y;
    while (k--) {
        scanf("%d%d%d", &op, &x, &y);
        if (x > n || y > n) res++;
        else {
            int px = find(x), py = find(y);
            if (op == 1) {
                if (px == py && (d[x] - d[y]) % 3) {
                    res++;
                } else if (px != py) {
                    p[px] = py;
                    d[px] = d[y] - d[x];
                }
            } else {
                if (px == py && (d[x] - d[y] - 1) % 3) {
                    res++;
                } else if (px != py) {
                    p[px] = py;
                    d[px] = d[y] - d[x] + 1;
                }
            }
        }
    }
    printf("%d\n", res);
    return 0;
}
```

---

[Codeforces 371 D. Vessels](https://codeforces.com/contest/371/problem/D)

