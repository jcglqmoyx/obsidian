## [Codeforces 1661 C. Water the Trees](https://codeforces.com/contest/1661/problem/C)
There are 𝑛 trees in a park, numbered from 11 to 𝑛. The initial height of the 𝑖-th tree is ℎ𝑖.

You want to water these trees, so they all grow to the same height.

The watering process goes as follows. You start watering trees at day 1. During the 𝑗-th day you can:

-   Choose a tree and water it. If the day is odd (e.g. 1,3,5,7,…), then the height of the tree increases by 1. If the day is even (e.g. 2,4,6,8,…), then the height of the tree increases by 2.
-   Or skip a day without watering any tree.

Note that you can't water more than one tree in a day.

Your task is to determine the minimum number of days required to water the trees so they grow to the same height.

You have to answer 𝑡 independent test cases.

Input

The first line of the input contains one integer 𝑡 (1≤𝑡≤2⋅1e4) — the number of test cases.

The first line of the test case contains one integer 𝑛 (1≤𝑛≤3e5) — the number of trees.

The second line of the test case contains 𝑛 integers ℎ1,ℎ2,…,ℎ𝑛 (1≤ℎ𝑖≤1e9), where ℎ𝑖 is the height of the 𝑖-th tree.

It is guaranteed that the sum of 𝑛 over all test cases does not exceed 3⋅1e5 (∑𝑛≤3⋅105).

Output

For each test case, print one integer — the minimum number of days required to water the trees, so they grow to the same height.

Example

### input

```
3
3
1 2 4
5
4 4 3 5 5
7
2 5 4 8 3 7 4
```


### output
```
4
3
16
```

### Note

Consider the first test case of the example. The initial state of the trees is [1,2,4][1,2,4].

1.  During the first day, let's water the first tree, so the sequence of heights becomes [2,2,4][2,2,4];
2.  during the second day, let's water the second tree, so the sequence of heights becomes [2,4,4][2,4,4];
3.  let's skip the third day;
4.  during the fourth day, let's water the first tree, so the sequence of heights becomes [4,4,4][4,4,4].

Thus, the answer is 4.
```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
using LL = long long;  
  
#define rep(i, a, b) for (int (i) = (a); (i) < (b); (i)++)  
  
const int N = 3e5 + 10;  
  
int n;  
int h[N];  
  
void solve() {  
    scanf("%d", &n);  
    rep(i, 0, n) scanf("%d", &h[i]);  
    int mx = *max_element(h, h + n);  
    LL res = INT64_MAX;  
    for (int x: {mx, mx + 1}) {
        LL odd = 0, even = 0;  
        LL days = 0;  
        rep(i, 0, n) {  
            int d = x - h[i];
            even += d / 2;  
            if (d & 1) odd++;  
        }  
        if (odd == even) days = odd * 2;  
        else if (odd < even) {  
            even = (even - odd) * 2;  
            days = odd * 2 + even / 3 * 2 + even % 3;  
        } else {  
            days = odd * 2 - 1;  
        }  
        res = min(res, days);  
    }  
    printf("%lld\n", res);  
}  
  
int main() {  
    int T;  
    scanf("%d", &T);  
    while (T--) solve(); 
    return 0;  
}
```