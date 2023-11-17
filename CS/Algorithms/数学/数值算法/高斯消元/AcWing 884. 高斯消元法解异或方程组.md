## [AcWing 884. 高斯消元解异或线性方程组](https://www.acwing.com/problem/content/description/886/)

输入一个包含 n 个方程 n 个未知数的异或线性方程组。

方程组中的系数和常数为 0 或 1，每个未知数的取值也为 0 或 1。

求解这个方程组。

异或线性方程组示例如下：

```markdown
M[1][1]x[1] ^ M[1][2]x[2] ^ … ^ M[1][n]x[n] = B[1]
M[2][1]x[1] ^ M[2][2]x[2] ^ … ^ M[2][n]x[n] = B[2]
…
M[n][1]x[1] ^ M[n][2]x[2] ^ … ^ M[n][n]x[n] = B[n]

```

其中 `^` 表示异或(XOR)，M[i][j] 表示第 i 个式子中 x[j] 的系数，B[i] 是第 i 个方程右端的常数，取值均为 0 或 1。

### **输入格式**

第一行包含整数 n。

接下来 n 行，每行包含 n+1 个整数  或 1，表示一个方程的 n 个系数以及等号右侧的常数。

### **输出格式**

如果给定线性方程组存在唯一解，则输出共 nn 行，其中第 ii 行输出第 ii 个未知数的解。

如果给定线性方程组存在多组解，则输出 `Multiple sets of solutions`。

如果给定线性方程组无解，则输出 `No solution`。

### **数据范围**

1 ≤ n ≤ 100

### **输入样例：**

```
3
1 1 0 1
0 1 1 0
1 0 0 1
```

### **输出样例：**

```
1
0
0
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 110;

int n;
int a[N][N];

int gauss() {
    int c, r;
    for (c = 0, r = 0; c < n; c++) {
        int t = r;
        for (int i = r; i < n; i++) {
            if (a[i][c]) t = i;
        }

        if (!a[t][c]) continue;

        for (int i = c; i <= n; i++) swap(a[r][i], a[t][i]);
        for (int i = r + 1; i < n; i++) {
            if (a[i][c]) {
                for (int j = n; j >= c; j--) {
                    a[i][j] ^= a[r][j];
                }
            }
        }
        r++;
    }

    if (r < n) {
        for (int i = r; i < n; i++) {
            if (a[i][n]) return 2;
        }
        return 1;
    }

    for (int i = n - 1; i >= 0; i--) {
        for (int j = i + 1; j < n; j++) {
            a[i][n] ^= a[i][j] * a[j][n];
        }
    }

    return 0;
}

int main() {
    cin >> n;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n + 1; j++) {
            cin >> a[i][j];
        }
    }

    int t = gauss();

    if (t == 0) {
        for (int i = 0; i < n; i++) {
            cout << a[i][n] << endl;
        }
    } else if (t == 1) {
        puts("Multiple sets of solutions");
    } else {
        puts("No solution");
    }

    return 0;
}
```