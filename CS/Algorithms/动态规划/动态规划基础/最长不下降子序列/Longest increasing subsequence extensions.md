## AcWing **1010. 拦截导弹**

某国为了防御敌国的导弹袭击，发展出一种导弹拦截系统。

但是这种导弹拦截系统有一个缺陷：虽然它的第一发炮弹能够到达任意的高度，但是以后每一发炮弹都不能高于前一发的高度。

某天，雷达捕捉到敌国的导弹来袭。

由于该系统还在试用阶段，所以只有一套系统，因此有可能不能拦截所有的导弹。

输入导弹依次飞来的高度（雷达给出的高度数据是不大于30000的正整数，导弹数不超过1000），计算这套系统最多能拦截多少导弹，如果要拦截所有导弹最少要配备多少套这种导弹拦截系统。

### **输入格式**

共一行，输入导弹依次飞来的高度。

### **输出格式**

第一行包含一个整数，表示最多能拦截的导弹数。

第二行包含一个整数，表示要拦截所有导弹最少要配备的系统数。

### **数据范围**

雷达给出的高度数据是不大于 30000 的正整数，导弹数不超过 1000。

### **输入样例：**

```
389 207 155 300 299 170 158 65
```

### **输出样例：**

```
6
2
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1005;

int f[N], g[N];

int main() {
    int len = 0, cnt = 0;
    int a;
    while (cin >> a) {
        auto pos1 = upper_bound(f, f + len, a, greater<>()) - f;
        if (pos1 == len) f[len++] = a;
        else f[pos1] = a;

        auto pos2 = lower_bound(g, g + cnt, a) - g;
        if (pos2 == cnt) g[cnt++] = a;
        else g[pos2] = a;
    }
    cout << len << endl;
    cout << cnt << endl;
    return 0;
}
```

## [Acwing **187. 导弹防御系统**](https://www.acwing.com/problem/content/description/189/)

为了对抗附近恶意国家的威胁，RR 国更新了他们的导弹防御系统。

一套防御系统的导弹拦截高度要么一直 **严格单调** 上升要么一直 **严格单调** 下降。

例如，一套系统先后拦截了高度为 3 和高度为 4 的两发导弹，那么接下来该系统就只能拦截高度大于 4 的导弹。

给定即将袭来的一系列导弹的高度，请你求出至少需要多少套防御系统，就可以将它们全部击落。

### **输入格式**

输入包含多组测试用例。

对于每个测试用例，第一行包含整数 n，表示来袭导弹数量。

第二行包含 n 个**不同的**整数，表示每个导弹的高度。

当输入测试用例 n = 0 时，表示输入终止，且该用例无需处理。

### **输出格式**

对于每个测试用例，输出一个占据一行的整数，表示所需的防御系统数量。

### **数据范围**

1 ≤ n ≤ 50

### **输入样例：**

```
5
3 5 2 4 1
0
```

### **输出样例：**

```
2
```

### **样例解释**

对于给出样例，最少需要两套防御系统。

一套击落高度为 3, 4 的导弹，另一套击落高度为 5, 2, 1 的导弹。

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 55;

int n;
int a[N];
int up[N], down[N];
int res;

void dfs(int u, int su, int sd) {
    if (su + sd >= res) return;
    if (u == n) {
        res = su + sd;
        return;
    }
    int k = 0;
    while (k < su && a[u] >= up[k]) k++;
    int t = up[k];
    up[k] = a[u];
    if (k >= su) dfs(u + 1, su + 1, sd);
    else dfs(u + 1, su, sd);
    up[k] = t;

    k = 0;
    while (k < sd && a[u] <= down[k]) k++;
    t = down[k];
    down[k] = a[u];
    if (k >= sd) dfs(u + 1, su, sd + 1);
    else dfs(u + 1, su, sd);
    down[k] = t;
}

int main() {
    while (cin >> n, n) {
        for (int i = 0; i < n; i++) cin >> a[i];
        res = n;
        dfs(0, 0, 0);
        cout << res << endl;
    }
    return 0;
}
```