---
page-title: AcWing 1282. 搜索关键词
url: https://www.acwing.com/problem/content/1284/
date: 2024-05-01 16:48:15
---
给定 n𝑛 个长度不超过 5050 的由小写英文字母组成的单词，以及一篇长为 m𝑚 的文章。

请问，其中有多少个单词在文章中出现了。

**注意：每个单词不论在文章中出现多少次，仅累计 11 次。**

#### 输入格式

第一行包含整数 T𝑇，表示共有 T𝑇 组测试数据。

对于每组数据，第一行一个整数 n𝑛，接下去 n𝑛 行表示 n𝑛 个单词，最后一行输入一个字符串，表示文章。

#### 输出格式

对于每组数据，输出一个占一行的整数，表示有多少个单词在文章中出现。

#### 数据范围

1≤T≤201≤𝑇≤20,  
1≤n≤1041≤𝑛≤104,  
1≤m≤1061≤𝑚≤106,  
一个测试点内，所有 𝑛 相加之和不超过 10^4，所有 m𝑚 相加之和不超过 10^6。

#### 输入样例：

```
1
5
she
he
say
shr
her
yasherhs
```

#### 输出样例：

```
3
```

```cpp
#pragma GCC optimize(2)

#include <bits/stdc++.h>

using namespace std;

const int N = 55, M = 1000010;

int n;
char word[N];
char text[M];
int tr[N * 10000][26], idx;
int cnt[N * 10000];
int ne[N * 10000];
int q[N * 10000];

void insert() {
    int p = 0;
    for (int i = 0; word[i]; i++) {
        int u = word[i] - 'a';
        if (!tr[p][u]) tr[p][u] = ++idx;
        p = tr[p][u];
    }
    cnt[p]++;
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
    idx = 0;
    memset(cnt, 0, sizeof cnt);
    memset(tr, 0, sizeof tr);
    memset(ne, 0, sizeof ne);
    scanf("%d", &n);
    while (n--) {
        scanf("%s", word);
        insert();
    }
    build();
    scanf("%s", text);
    int res = 0;
    for (int p = 0, i = 0; text[i]; i++) {
        p = tr[p][text[i] - 'a'];
        for (int j = p; j && ~cnt[j]; j = ne[j]) {
            res += cnt[j], cnt[j] = -1;
        }
    }
    printf("%d\n", res);
}

int main() {
    int testcases;
    scanf("%d", &testcases);
    while (testcases--) solve();
    return 0;
}
```