## [AcWing 883. 高斯消元解线性方程组](https://www.acwing.com/problem/content/885/)

输入一个包含 n 个方程 n 个未知数的线性方程组。

方程组中的系数为实数。

求解这个方程组。

下图为一个包含 m 个方程 n 个未知数的线性方程组示例：

![https://cdn.acwing.com/media/article/image/2019/06/27/19_b30080c698-9a504fc2d5628535be9dcb5f90ef76c6a7ef634a.gif](https://cdn.acwing.com/media/article/image/2019/06/27/19_b30080c698-9a504fc2d5628535be9dcb5f90ef76c6a7ef634a.gif)

### **输入格式**

第一行包含整数 n。

接下来 n 行，每行包含 n + 1 个实数，表示一个方程的 n 个系数以及等号右侧的常数。

### **输出格式**

如果给定线性方程组存在唯一解，则输出共 n 行，其中第 i 行输出第 i 个未知数的解，结果保留两位小数。

如果给定线性方程组存在无数解，则输出 `Infinite group solutions`。

如果给定线性方程组无解，则输出 `No solution`。

### **数据范围**

1 ≤ n ≤ 100,

所有输入系数以及常数均保留两位小数，绝对值均不超过 100。

### **输入样例：**

```css
3
1.00 2.00 -1.00 -6.00
2.00 1.00 -3.00 -9.00
-1.00 -1.00 2.00 7.00
```

### **输出样例：**

```css
1.00
-2.00
3.00
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 110;
const double eps = 1e-8;

int n;
double a[N][N];

// 高斯消元，答案存于a[i][n]中，0 <= i < n
int gauss() {
    int c, r;
    for (c = 0, r = 0; c < n; c++) {
        int t = r;
        // 找绝对值最大的行
        for (int i = r; i < n; i++) {
            if (fabs(a[i][c]) > fabs(a[t][c])) t = i;
        }

        if (fabs(a[t][c]) < eps) continue;

        for (int i = c; i <= n; i++) swap(a[t][i], a[r][i]);  // 将绝对值最大的行换到最顶端
        for (int i = n; i >= c; i--) a[r][i] /= a[r][c];  // 将当前行的首位变成1

        // 用当前行将下面所有的列消成0
        for (int i = r + 1; i < n; i++) {
            if (fabs(a[i][c]) > eps) {
                for (int j = n; j >= c; j--) {
                    a[i][j] -= a[r][j] * a[i][c];
                }
            }
        }
        r++;
    }

    if (r < n) {
        for (int i = r; i < n; i++) {
            if (fabs(a[i][n]) > eps) return 2; // 无解
        }
        return 1; // 有无穷多组解
    }

    for (int i = n - 1; i >= 0; i--) {
        for (int j = i + 1; j < n; j++) {
            a[i][n] -= a[i][j] * a[j][n];
        }
    }

    return 0; // 有唯一解
}

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n + 1; j++) {
            scanf("%lf", &a[i][j]);
        }
    }

    int t = gauss();
    if (t == 2) puts("No solution");
    else if (t == 1) puts("Infinite group solutions");
    else {
        for (int i = 0; i < n; i++) {
            if (fabs(a[i][n]) < eps) a[i][n] = 0;  // 去掉输出 -0.00 的情况
            printf("%.2lf\n", a[i][n]);
        }
    }

    return 0;
}
```