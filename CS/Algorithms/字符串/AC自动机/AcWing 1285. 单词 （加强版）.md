---
page-title: AcWing 1285. 单词
url: https://www.acwing.com/problem/content/1287/
date: 2024-05-01 16:50:14
---
某人读论文，一篇论文是由许多单词组成的。

但他发现一个单词会在论文中出现很多次，现在他想知道每个单词分别在论文中出现多少次。

#### 输入格式

第一行一个整数 N𝑁，表示有多少个单词。

接下来 N𝑁 行每行一个单词，单词中只包含小写字母。

#### 输出格式

输出 N𝑁 个整数，每个整数占一行，第 i𝑖 行的数字表示第 i𝑖 个单词在文章中出现了多少次。

#### 数据范围

1≤N≤2001≤𝑁≤200,  
所有单词长度的总和不超过 106106。

#### 输入样例：

```
3
a
aa
aaa
```

#### 输出样例：

```
6
3
1
```

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1000010;

int n;
char s[N];
int tr[N][26], idx;
int ne[N];
int cnt[N]; 
int q[N];
int id[205];

void insert(int a) {
   int p = 0;
   for (int i = 0; s[i]; i++) {
       int u = s[i] - 'a';
       if (!tr[p][u]) tr[p][u] = ++idx;
       p = tr[p][u];
       cnt[p]++;
   }
   id[a] = p;
}

void build() {
    int hh = 0, tt = -1;
    for (int i = 0; i < 26; i++) {
        if (tr[0][i]) {
            q[++tt] = tr[0][i];
        }
    }
    while (hh <= tt) {
        int t = q[hh++];
        for (int i = 0; i < 26; i++) {
            if (tr[t][i]) {
                ne[tr[t][i]] = tr[ne[t]][i];
                q[++tt] = tr[t][i];
            } else {
                tr[t][i] = tr[ne[t]][i];
            }
        }
    }
}

void solve() {
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%s", s); 
        insert(i);
    }
    build();
    for (int i = idx - 1; i >= 0; i--) cnt[ne[q[i]]] += cnt[q[i]];
    for (int i = 0; i < n; i++) printf("%d\n", cnt[id[i]]);
}

int main() {
    solve();
    return 0;
}
```