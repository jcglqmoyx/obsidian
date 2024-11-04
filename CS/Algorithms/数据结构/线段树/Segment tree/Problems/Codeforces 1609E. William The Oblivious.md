# William The Oblivious

## 题面翻译

给你一个长为 $n$ 的字符串，只包含 `abc` 三种字符。$q$ 次操作，每次把一个位置的字符改成给定字符，询问当前串至少修改几次满足不包含子序列 `abc`。修改指把一个位置的字符修改成 `a`、`b`、`c` 三种字符之一。

$1\le n,q\le 10^5$。

## 输入格式

The first line contains two integers $n$ and $q$ $(1 \le n, q \le 10^5)$ , the length of the string and the number of queries, respectively.

The second line contains the string $s$ , consisting of characters "a", "b" and "c".

Each of the next $q$ lines contains an integer $i$ and character $c$ $(1 \le i \le n)$ , index and the value of the new item in the string, respectively. It is guaranteed that character's $ c $ value is "a", "b" or "c".

## 输出格式

For each query output the minimal number of characters that would have to be replaced so that the string doesn't contain "abc" as a subsequence.

## 样例 #1

### 样例输入 #1

```
9 12
aaabccccc
4 a
4 b
2 b
5 a
1 b
6 b
5 c
2 a
1 a
5 a
6 b
7 b
```

### 样例输出 #1

```
0
1
2
2
1
2
1
2
2
2
2
2
```

## 提示

Let's consider the state of the string after each query:

1. $s =$ "aaaaccccc". In this case the string does not contain "abc" as a subsequence and no replacements are needed.
2. $s =$ "aaabccccc". In this case 1 replacements can be performed to get, for instance, string $ s =  $ "aaaaccccc". This string does not contain "abc" as a subsequence.
3. $s =$ "ababccccc". In this case 2 replacements can be performed to get, for instance, string $ s =  $ aaaaccccc". This string does not contain "abc" as a subsequence.
4. $s =$ "ababacccc". In this case 2 replacements can be performed to get, for instance, string $ s =  $ "aaaaacccc". This string does not contain "abc" as a subsequence.
5. $s =$ "bbabacccc". In this case 1 replacements can be performed to get, for instance, string $ s =  $ "bbacacccc". This string does not contain "abc" as a subsequence.
6. $s =$ "bbababccc". In this case 2 replacements can be performed to get, for instance, string $ s =  $ "bbacacccc". This string does not contain "abc" as a subsequence.
7. $s =$ "bbabcbccc". In this case 1 replacements can be performed to get, for instance, string $ s =  $ "bbcbcbccc". This string does not contain "abc" as a subsequence.
8. $s =$ "baabcbccc". In this case 2 replacements can be performed to get, for instance, string $ s =  $ "bbbbcbccc". This string does not contain "abc" as a subsequence.
9. $s =$ "aaabcbccc". In this case 2 replacements can be performed to get, for instance, string $ s =  $ "aaacccccc". This string does not contain "abc" as a subsequence.
10. $s =$ "aaababccc". In this case 2 replacements can be performed to get, for instance, string $ s =  $ "aaacacccc". This string does not contain "abc" as a subsequence.
11. $s =$ "aaababccc". In this case 2 replacements can be performed to get, for instance, string $ s =  $ "aaacacccc". This string does not contain "abc" as a subsequence.
12. $s =$ "aaababbcc". In this case 2 replacements can be performed to get, for instance, string $ s =  $ "aaababbbb". This string does not contain "abc" as a subsequence.


```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
const int N = 100010;  
  
int n;  
char s[N];  
  
struct Node {  
    int l, r;  
    int cnt[3];  
    int fab, fbc, fabc;  
} tr[N << 2];  
  
void push_up(int u) {  
    auto &root = tr[u], &l = tr[u << 1], &r = tr[u << 1 | 1];  
    for (int i = 0; i < 3; i++) {  
        root.cnt[i] = l.cnt[i] + r.cnt[i];  
    }  
    root.fab = min({l.cnt[0] + r.fab, l.fab + r.cnt[1]});  
    root.fbc = min({l.cnt[1] + r.fbc, l.fbc + r.cnt[2]});  
    root.fabc = min({l.fabc + r.cnt[2], l.cnt[0] + r.fabc, l.fab + r.fbc});  
}  
  
void build(int u, int l, int r) {  
    tr[u] = {l, r};  
    if (l == r) {  
        for (int i = 0; i < 3; i++) {  
            tr[u].cnt[i] = (s[l] - 'a') == i;  
        }  
    } else {  
        int mid = (l + r) >> 1;  
        build(u << 1, l, mid);  
        build(u << 1 | 1, mid + 1, r);  
        push_up(u);  
    }  
}  
  
void modify(int u, int p, char c) {  
    if (tr[u].l == tr[u].r) {  
        tr[u].cnt[s[p] - 'a']--;  
        tr[u].cnt[c - 'a']++;  
        s[p] = c;  
    } else {  
        int mid = (tr[u].l + tr[u].r) >> 1;  
        if (p <= mid) modify(u << 1, p, c);  
        else modify(u << 1 | 1, p, c);  
        push_up(u);  
    }  
}  
  
void solve() {  
    int q;  
    scanf("%d%d", &n, &q);  
    scanf("%s", s + 1);  
    build(1, 1, n);  
    while (q--) {  
        int i;  
        char c;  
        scanf("%d", &i);  
        scanf(" %c", &c);  
        modify(1, i, c);  
        printf("%d\n", tr[1].fabc);  
    }  
}  
  
int main() {  
    solve();  
    return 0;  
}
```