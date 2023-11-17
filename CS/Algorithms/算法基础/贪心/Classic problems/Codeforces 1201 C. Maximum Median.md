## [Codeforces 1201 C. Maximum Median](https://codeforces.com/contest/1201/problem/C)
You are given an array 𝑎 of 𝑛 integers, where 𝑛 is odd. You can make the following operation with it:

-   Choose one of the elements of the array (for example 𝑎𝑖) and increase it by 1 (that is, replace it with 𝑎𝑖+11).

You want to make the median of the array the largest possible using at most 𝑘 operations.

The median of the odd-sized array is the middle element after the array is sorted in non-decreasing order. For example, the median of the array [1,5,2,3,5] is 33.

### Input 

The first line contains two integers 𝑛 and 𝑘 (1≤𝑛≤2⋅100000, 𝑛 is odd, 1≤𝑘≤1e9) — the number of elements in the array and the largest number of operations you can make.

The second line contains 𝑛 integers 𝑎1,𝑎2,…,$a_n$ (1≤𝑎𝑖≤1e9).

### Output

Print a single integer — the maximum possible median after the operations.
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
#define rep(i, a, b) for (int (i) = (a); (i) < (b); (i)++)  
  
const int N = 200010;  
  
int n, k;  
int a[N];  
  
void solve() {  
    scanf("%d%d", &n, &k);  
    rep(i, 0, n) scanf("%d", &a[i]);  
    sort(a, a + n);  
    int i = n / 2 + 1;  
    for (; i < n; i++) {  
        if (a[i] != a[i - 1]) {  
            if (a[i] - a[i - 1] > k / (i - n / 2)) break;  
            k -= (i - n / 2) * (a[i] - a[i - 1]);  
        }  
    }  
    int res = a[i - 1] + k / (i - n / 2);  
    printf("%d\n", res);  
}  
  
int main() {  
    solve();  
    return 0;  
}
```