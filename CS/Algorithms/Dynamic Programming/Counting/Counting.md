## [AcWing **900. 整数划分**](https://www.acwing.com/problem/content/description/902/)

一个正整数 n 可以表示成若干个正整数之和，形如：n = $n_1$ + $n_2$ + … + $n_k$，其中 $n_1$ ≥ $n_2$ ≥ … ≥ $n_k$, k ≥ 1。

我们将这样的一种表示称为正整数 n 的一种划分。

现在给定一个正整数 n，请你求出 n 共有多少种不同的划分方法。

### **输入格式**

共一行，包含一个整数 n。

### **输出格式**

共一行，包含一个整数，表示总划分数量。

由于答案可能很大，输出结果请对 $10^9+7$ 取模。

### **数据范围**

1 ≤ n ≤ 1000

### **输入样例:**

```
5
```

### **输出样例：**

```
7
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1005, MOD = 1e9 + 7;

int n;
int f[N];

int main() {
    cin >> n;
    f[0] = 1;
    for (int i = 1; i <= n; i++) {
        for (int j = i; j <= n; j++) {
            f[j] = (f[j] + f[j - i]) % MOD;
        }
    }
    cout << f[n] << endl;
    return 0;
}
```


----

[CF163A](https://codeforces.com/problemset/problem/163/A)
[Codeforces 1426F](https://codeforces.com/problemset/problem/1426/F)
[LeetCode 1638. Count Substrings That Differ by One Character](https://leetcode.com/problems/count-substrings-that-differ-by-one-character/description/)
## [2638. Count the Number of K-Free Subsets](https://leetcode.cn/problems/count-the-number-of-k-free-subsets/)
---
You are given an integer array `nums`, which contains **distinct** elements and an integer `k`.

A subset is called a **k-Free** subset if it contains **no** two elements with an absolute difference equal to `k`. Notice that the empty set is a **k-Free** subset.

Return *the number of **k-Free** subsets of* `nums`.

A **subset** of an array is a selection of elements (possibly none) of the array.

**Example 1:**

**Input:** nums = \[5,4,6\], k = 1
**Output:** 5
**Explanation:** There are 5 valid subsets: {}, {5}, {4}, {6} and {4, 6}.

**Example 2:**

**Input:** nums = \[2,3,5,8\], k = 5
**Output:** 12
**Explanation:** There are 12 valid subsets: {}, {2}, {3}, {5}, {8}, {2, 3}, {2, 3, 5}, {2, 5}, {2, 5, 8}, {2, 8}, {3, 5} and {5, 8}.

**Example 3:**

**Input:** nums = \[10,5,9,11\], k = 20
**Output:** 16
**Explanation:** All subsets are valid. Since the total count of subsets is 24 \= 16, so the answer is 16. 

**Constraints:**

-   `1 <= nums.length <= 50`
-   `1 <= nums[i] <= 1000`
-   `1 <= k <= 1000`
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    long long countTheNumOfKFreeSubsets(vector<int> &nums, int k) {  
        sort(nums.begin(), nums.end());  
        unordered_map<int, vector<int>> map;  
        for (int x: nums) map[x % k].push_back(x);  
        long long res = 1;  
        for (auto &[_, v]: map) {  
            long long f0 = 1, f1 = 1;  
            int last = v[0];  
            for (int i = 1; i < v.size(); i++) {  
                int x = v[i];  
                long long g0 = f0 + f1, g1 = f0 + f1;  
                if (x - last == k) g1 = f0;  
                f0 = g0, f1 = g1;  
                last = x;  
            }  
            res *= f0 + f1;  
        }  
        return res;  
    }  
};
```