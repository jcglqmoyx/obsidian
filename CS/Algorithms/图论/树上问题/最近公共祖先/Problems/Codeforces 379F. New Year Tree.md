# New Year Tree

## 题面翻译

- 有一颗 $4$ 个节点的树，$2,3,4$ 号节点的父亲节点都是 $1$。$m$ 次操作。操作有 $1$ 种：
	1. `u`：设现在这颗树有 $n$ 个节点，则您要新建两个节点 $n+1,n+2$ 并让它们变为 $u$ 的儿子节点（连一条 $u$ 到 $n+1$ 的边和 $u$ 到 $n+2$ 的边）。**保证 $u$ 是叶子节点**。进行完操作后，您要输出此时树的直径。
- $1\le m\le 5\times 10^5$。

## 题目描述

You are a programmer and you have a New Year Tree (not the traditional fur tree, though) — a tree of four vertices: one vertex of degree three (has number 1), connected with three leaves (their numbers are from 2 to 4).

On the New Year, programmers usually have fun. You decided to have fun as well by adding vertices to the tree. One adding operation looks as follows:

- First we choose some leaf of the tree with number $ v $ .
- Let's mark the number of vertices on the tree at this moment by variable $ n $ , then two vertexes are added to the tree, their numbers are $ n+1 $ and $ n+2 $ , also you get new edges, one between vertices $ v $ and $ n+1 $ and one between vertices $ v $ and $ n+2 $ .

Your task is not just to model the process of adding vertices to the tree, but after each adding operation print the diameter of the current tree. Come on, let's solve the New Year problem!

## 输入格式

The first line contains integer $ q $ $ (1<=q<=5·10^{5}) $ — the number of operations. Each of the next $ q $ lines contains integer $ v_{i} $ $ (1<=v_{i}<=n) $ — the operation of adding leaves to vertex $ v_{i} $ . Variable $ n $ represents the number of vertices in the current tree.

It is guaranteed that all given operations are correct.

## 输出格式

Print $ q $ integers — the diameter of the current tree after each operation.

## 样例 #1

### 样例输入 #1

```
5
2
3
4
8
5
```

### 样例输出 #1

```
3
4
4
5
6
```

## 算法

注意到，每次增加一个节点，直径长度至多增加 1，并且直径的两个端点，其中一个不变。  <br>维护直径的两个端点 end1 和 end2，以及直径长度 diameter。  <br>初始值 end1=2, end2=3, diameter=2。  <br>每次添加节点 cur 时，计算 cur 到端点的距离 dis(end1, cur) 和 dis(end2, cur)，如果其中有一个距离大于 diameter，那么更新 diameter 和直径端点。

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
const int N = 1000010, M = 19;  
  
int d[N];  
int p[N][19];  
  
int lca(int a, int b) {  
    if (d[a] > d[b]) swap(a, b);  
    for (int i = M - 1; i >= 0; i--) {  
        if (d[b] - (1 << i) >= d[a]) {  
            b = p[b][i];  
        }  
    }  
    if (a == b) return a;  
    for (int i = M - 1; i >= 0; i--) {  
        if (p[a][i] != p[b][i]) {  
            a = p[a][i];  
            b = p[b][i];  
        }  
    }  
    return p[a][0];  
}  
  
void solve() {  
    for (int node = 2; node <= 4; node++) {  
        d[node] = 1;  
    }  
    int end1 = 2, end2 = 3;  
    int diameter = 2;  
    int q, v;  
    scanf("%d", &q);  
    int n = 4;  
    while (q--) {  
        scanf("%d", &v);  
        int a = n + 1, b = n + 2;  
        p[a][0] = p[b][0] = v;  
        d[a] = d[b] = d[v] + 1;  
        for (int i = 1; i < M; i++) {  
            int t = p[p[a][i - 1]][i - 1];  
            if (t) p[a][i] = p[b][i] = t;  
            else break;  
        }  
        int dist = d[end1] + d[a] - d[lca(a, end1)] * 2;  
        if (dist > diameter) {  
            diameter = dist;  
            end2 = end1;  
            end1 = a;  
        }  
        dist = d[end2] + d[a] - d[lca(a, end2)] * 2;  
        if (dist > diameter) {  
            diameter = dist;  
            end1 = end2;  
            end2 = b;  
        }  
        printf("%d\n", diameter);  
        n += 2;  
    }  
}  
  
int main() {  
    solve();  
    return 0;  
}
```
