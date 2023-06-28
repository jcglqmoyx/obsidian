# Longest Increasing Subsequence

## [AcWing **895. 最长上升子序列**](https://www.acwing.com/problem/content/description/897/)

给定一个长度为 N 的数列，求数值严格单调递增的子序列的长度最长是多少。

### **输入格式**

第一行包含整数 N。

第二行包含 N 个整数，表示完整序列。

### **输出格式**

输出一个整数，表示最大长度。

### **数据范围**

1 ≤ N ≤ 1000，

−$10^9$ ≤ 数列中的数 ≤ $10^9$

### **输入样例：**

```
7
3 1 2 1 8 5 6
```

### **输出样例：**

```
4
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1010;

int n;
int w[N], f[N];

int main() {
    cin >> n;
    for (int i = 0; i < n; i++) cin >> w[i];

    int mx = 1;
    for (int i = 0; i < n; i++) {
        f[i] = 1;
        for (int j = 0; j < i; j++) {
            if (w[i] > w[j]) f[i] = max(f[i], f[j] + 1);
        }
        mx = max(mx, f[i]);
    }

    cout << mx << endl;
    return 0;
}
```

## [AcWing **896. 最长上升子序列 II**](https://www.acwing.com/problem/content/898/)

给定一个长度为 N 的数列，求数值严格单调递增的子序列的长度最长是多少。

### **输入格式**

第一行包含整数 N。

第二行包含 N 个整数，表示完整序列。

### **输出格式**

输出一个整数，表示最大长度。

### **数据范围**

1 ≤ N ≤ 100000，

−$10^9$ ≤ 数列中的数 ≤ $10^9$

### **输入样例：**

```
7
3 1 2 1 8 5 6
```

### **输出样例：**

```
4
```

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
    int n;
    cin >> n;
    vector<int> f;
    while (n--) {
        int num;
        cin >> num;
        if (f.empty() || num > f.back()) {
            f.push_back(num);
        } else {
            auto p = lower_bound(f.begin(), f.end(), num);
            *p = num;
        }
    }
    cout << f.size() << endl;
    return 0;
}
```