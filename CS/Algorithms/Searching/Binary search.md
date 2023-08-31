### [See Wikipedia](https://en.wikipedia.org/wiki/Binary_search_algorithm)

---

[AcWing 789](https://www.acwing.com/problem/content/791/)     [LeetCode 704](https://leetcode.cn/problems/binary-search/)

## [AcWing 789](https://www.acwing.com/problem/content/791/) （区间二分）

给定一个按照升序排列的长度为 $n$ 的整数数组，以及 $q$ 个查询。

对于每个查询，返回一个元素 $k$ 的起始位置和终止位置（位置从 $0$ 开始计数）。

如果数组中不存在该元素，则返回 `-1 -1`。

### **输入格式**

第一行包含整数 $n$ 和 $q$, 表示数组长度和询问个数。

第二行包含 $n$ 个整数（均在 1 ∼ 10000 范围内），表示完整数组。

接下来 $q$ 行，每行包含一个整数 $k$，表示一个询问元素。

### **输出格式**

共 $q$ 行，每行包含两个整数，表示所求元素的起始位置和终止位置。

如果数组中不存在该元素，则返回 `-1 -1`。

### **数据范围**

1≤n≤1000001≤n≤1000001≤q≤100001≤q≤100001≤k≤100001≤k≤10000

### **输入样例：**

```
6 3
1 2 2 3 3 4
3
4
5
```

### **输出样例：**

```diff
3 4
5 5
-1 -1
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 100010;

int a[N];

int binary_search1(int l, int r, int k) {
    while (l < r) {
        int mid = (l + r) >> 1;
        if (a[mid] < k) l = mid + 1;
        else r = mid;
    }
    return a[l] == k ? l : -1;
}

int binary_search2(int l, int r, int k) {
    while (l < r) {
        int mid = (l + r + 1) >> 1;
        if (a[mid] <= k) l = mid;
        else r = mid - 1;
    }
    return l;
}

int main() {
    int n, q;
    cin >> n >> q;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }
    while (q--) {
        int k;
        cin >> k;
        int left = binary_search1(0, n - 1, k);
        if (left == -1) cout << "-1 -1" << endl;
        else {
            int right = binary_search2(0, n - 1, k);
            cout << left << ' ' << right << endl;
        }
    }
    return 0;
}
```

## [AcWing 790](https://www.acwing.com/problem/content/792/) (浮点数的二分）

给定一个浮点数 nn，求它的三次方根。

### **输入格式**

共一行，包含一个浮点数 nn。

### **输出格式**

共一行，包含一个浮点数，表示问题的解。

注意，结果保留 66 位小数。

### **数据范围**

−10000≤n≤10000−10000≤n≤10000

### **输入样例：**

```css
1000.00

```

### **输出样例：**

```css
10.000000
```

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
    double x;
    cin >> x;

    double l = -100, r = 100;
    while (r - l > 1e-8) {
        double mid = (l + r) / 2;
        if (mid * mid * mid >= x) r = mid;
        else l = mid;
    }

    printf("%.6lf\n", l);
    return 0;
}
```

## [LeetCode **2560. House Robber IV**](https://leetcode.cn/problems/house-robber-iv/)
## [LeetCode 2604. Minimum Time to Eat All Grains](https://leetcode.cn/problems/minimum-time-to-eat-all-grains/description/)
There are `n` hens and `m` grains on a line. You are given the initial positions of the hens and the grains in two integer arrays `hens` and `grains` of size `n` and `m` respectively.

Any hen can eat a grain if they are on the same position. The time taken for this is negligible. One hen can also eat multiple grains.

In `1` second, a hen can move right or left by `1` unit. The hens can move simultaneously and independently of each other.

Return _the **minimum** time to eat all grains if the hens act optimally._

**Example 1:**

**Input:** hens = [3,6,7], grains = [2,4,7,9]
**Output:** 2
**Explanation:** 
One of the ways hens eat all grains in 2 seconds is described below:
- The first hen eats the grain at position 2 in 1 second. 
- The second hen eats the grain at position 4 in 2 seconds. 
- The third hen eats the grains at positions 7 and 9 in 2 seconds. 
So, the maximum time needed is 2.
It can be proven that the hens cannot eat all grains before 2 seconds.

**Example 2:**

**Input:** hens = [4,6,109,111,213,215], grains = [5,110,214]
**Output:** 1
**Explanation:** 
One of the ways hens eat all grains in 1 second is described below:
- The first hen eats the grain at position 5 in 1 second. 
- The fourth hen eats the grain at position 110 in 1 second.
- The sixth hen eats the grain at position 214 in 1 second. 
- The other hens do not move. 
So, the maximum time needed is 1.

**Constraints:**

-   1 <= hens.length, grains.length <= 2*1e4
-   0 <= hens[i], grains[j] <= 1e9
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    int minimumTime(vector<int> &hens, vector<int> &grains) {  
        int n = (int) hens.size(), m = (int) grains.size();  
        sort(hens.begin(), hens.end()), sort(grains.begin(), grains.end());  
        auto check = [&](int mid) {  
            int j = 0;  
            for (int i = 0; i < n; i++) {  
                int l = 0, r = 0;  
                while (j < m) {  
                    if (hens[i] > grains[j]) l = max(l, hens[i] - grains[j]);  
                    else r = max(r, grains[j] - hens[i]);  
                    if (min(l * 2 + r, r * 2 + l) <= mid) j++;  
                    else break;
                }  
            }  
            return j == m;  
        };  
        int l = 0, r = 1.5e9;  
        while (l < r) {  
            int mid = l + (r - l) / 2;  
            if (check(mid)) r = mid;  
            else l = mid + 1;  
        }  
        return l;  
    }  
};
```