[cp-algorithms](https://cp-algorithms.com/num_methods/ternary_search.html)    [wikipedia](https://en.wikipedia.org/wiki/Ternary_search)

## [Codeforces 439D](https://codeforces.com/contest/439/problem/D)
Devu and his brother love each other a lot. As they are super geeks, they only like to play with arrays. They are given two arrays _a_ and _b_ by their father. The array _a_ is given to Devu and _b_ to his brother.

As Devu is really a naughty kid, he wants the minimum value of his array _a_ should be at least as much as the maximum value of his brother's array _b_.

Now you have to help Devu in achieving this condition. You can perform multiple operations on the arrays. In a single operation, you are allowed to decrease or increase any element of any of the arrays by 1. Note that you are allowed to apply the operation on any index of the array multiple times.

You need to find minimum number of operations required to satisfy Devu's condition so that the brothers can play peacefully without fighting.

### Input

The first line contains two space-separated integers _n_, _m_ (1 ≤ _n_, _m_ ≤ 1e5). The second line will contain _n_ space-separated integers representing content of the array _a_ (1 ≤ _$a_i$_ ≤ 1e9). The third line will contain _m_ space-separated integers representing content of the array _b_ (1 ≤ _$b_i$_ ≤ 1e9).

### Output

You need to output a single integer representing the minimum number of operations needed to satisfy Devu's condition.

### Examples

#### input
```
2 2  
2 3  
3 5  
```

#### output
```
3
```

#### input

```
3 2 
1 2 3
3 4
```
#### output
```
4
```

#### input

```
3 2  
4 5 6  
1 2
```
#### output
```
0
```
#### Note

In example 1, you can increase $a_1$ by 1 and decrease $b_2$ by 1 and then again decrease $b_2$ by 1. Now array _a_ will be [3; 3] and array _b_ will also be [3; 3]. Here minimum element of _a_ is at least as large as maximum element of _b_. So minimum number of operations needed to satisfy Devu's condition are 3.

In example 3, you don't need to do any operation, Devu's condition is already satisfied.
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
using LL = long long;  
  
#define rep(i, a, b) for (int (i) = (a); (i) < (b); (i)++)  
  
const int N = 1e5 + 10;  
  
int n, m;  
int a[N], b[N];  
  
LL f(int val) {  
    LL dt = 0;  
    rep(i, 1, n + 1) {  
        if (a[i] < val) dt += val - a[i];  
    }  
    rep(i, 1, m + 1) {  
        if (b[i] > val) dt += b[i] - val;  
    }  
    return dt;  
}  
  
void solve() {  
    scanf("%d%d", &n, &m);  
    rep(i, 1, n + 1) scanf("%d", &a[i]);  
    rep(i, 1, m + 1) scanf("%d", &b[i]);  
    int l = *min_element(a + 1, a + n + 1), r = *max_element(b + 1, b + m + 1);  
    LL da = 0, db = 0;  
    while (l < r) {  
        int m1 = l + (r - l) / 3;  
        int m2 = r - (r - l) / 3;  
        da = f(m1), db = f(m2);  
        if (da > db) {  
            l = m1 + 1;  
        } else {  
            r = m2 - 1;  
        }  
    }  
    printf("%lld", min(da, db));  
}  
  
int main() {  
    solve();  
    return 0;  
}
```