### [See Wikipedia](https://en.wikipedia.org/wiki/Merge_sort)

**Merge-sort a stable sort.**

---

Time complexity: $O(N*logN)$

Space complexity: $O(N)$ total with $O(N)$ auxiliary, $O(1)$ auxiliary with linked lists

---

[AcWing 787](https://www.acwing.com/problem/content/789/)      [LeetCode 912](https://leetcode.cn/problems/sort-an-array/)    [LeetCode 148(sorting linked list)](https://leetcode.cn/problems/sort-list/)

## Merge-sort

## T****op-down merge-sort****

### Sorting arrary

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1e5 + 5;

int a[N], tmp[N];

void merge_sort(int l, int r) {
    if (l >= r) return;
    int mid = (l + r) >> 1;
    merge_sort(l, mid), merge_sort(mid + 1, r);
    int k = 0, i = l, j = mid + 1;
    while (i <= mid && j <= r) {
        if (a[i] <= a[j]) tmp[k++] = a[i++];
        else tmp[k++] = a[j++];
    }
    while (i <= mid) tmp[k++] = a[i++];
    while (j <= r) tmp[k++] = a[j++];

    for (i = l, j = 0; i <= r; i++, j++) a[i] = tmp[j];
}

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }
    merge_sort(0, n - 1);
    for (int i = 0; i < n; i++) {
        cout << a[i] << " ";
    }
    return 0;
}
```

### Sorting linked list, $O(logN)$ space

```cpp
struct ListNode {
    int val;
    ListNode *next;

    ListNode() : val(0), next(nullptr) {}

    ListNode(int x) : val(x), next(nullptr) {}

    ListNode(int x, ListNode *next) : val(x), next(next) {}
};

class Solution {
public:
    ListNode *sortList(ListNode *head) {
        if (!head) return nullptr;
        int n = 0;
        for (auto p = head; p; p = p->next) n++;
        return sort(head, 0, n - 1);
    }

    ListNode *sort(ListNode *head, int low, int high) {
        if (low == high) return head;
        int mid = (low + high) >> 1;
        auto p = head;
        for (int i = 0; i < mid; i++) p = p->next;
        auto right_begin = p->next;
        p->next = nullptr;
        auto l = sort(head, low, mid);
        auto r = sort(right_begin, 0, high - mid - 1);
        auto dummy = new ListNode(), cur = dummy;
        while (l || r) {
            if (l && r) {
                if (l->val < r->val) cur->next = l, l = l->next;
                else cur->next = r, r = r->next;
            } else if (l) {
                cur->next = l, l = l->next;
            } else {
                cur->next = r, r = r->next;
            }
            cur = cur->next;
        }
        return dummy->next;
    }
};
```

## B****ottom-up merge-sort****

### Sorting array

```cpp
#include <bits/stdc++.h>

using namespace std;

vector<int> tmp;

void merge(vector<int> &a, int low, int mid, int high) {
    int i = low, j = mid + 1, idx = low;
    while (i <= mid && j <= high) {
        if (a[i] <= a[j]) tmp[idx++] = a[i++];
        else tmp[idx++] = a[j++];
    }
    while (i <= mid) tmp[idx++] = a[i++];
    while (j <= high) tmp[idx++] = a[j++];
    for (int k = low; k <= high; k++) a[k] = tmp[k];
}

void sort(vector<int> &a) {
    int n = (int) a.size();
    tmp.resize(n);
    for (int size = 1; size < n; size <<= 1) {
        for (int j = 0; j < n; j += size << 1) {
            merge(a, j, j + size - 1, min(j + (size << 1) - 1, n - 1));
        }
    }
}

int main() {
    vector<int> a = {3, 5, 4, 6, 1, 8, 2, 9, 7};
    printf("before sorting: ");
    for (int x: a) printf("%d ", x);
    printf("\n");
    sort(a);
    printf("after  sorting: ");
    for (int x: a) printf("%d ", x);
    return 0;
}
```

### Sorting linked list, $O(1)$ space

```cpp
struct ListNode {
    int val;
    ListNode *next;

    ListNode() : val(0), next(nullptr) {}

    ListNode(int x) : val(x), next(nullptr) {}

    ListNode(int x, ListNode *next) : val(x), next(next) {}
};

class Solution {
public:
    ListNode *merge(ListNode *l, ListNode *r) {
        auto dummy = new ListNode(), cur = dummy;
        while (l || r) {
            if (l && r) {
                if (l->val < r->val) cur->next = l, l = l->next;
                else cur->next = r, r = r->next;
            } else if (l) {
                cur->next = l, l = l->next;
            } else {
                cur->next = r, r = r->next;
            }
            cur = cur->next;
        }
        auto res = dummy->next;
        delete dummy;
        return res;
    }

    ListNode *sortList(ListNode *head) {
        if (!head) return nullptr;
        int n = 0;
        for (auto p = head; p; p = p->next) n++;
        for (int size = 1; size < n; size <<= 1) {
            auto dummy = new ListNode(-1, head), cur = head, last = dummy;
            for (int i = 0; i < n; i += size << 1) {
                auto p = cur, l = cur;
                for (int j = 0; j < size - 1; j++) {
                    if (p->next) p = p->next;
                }
                auto r = p->next;
                if (!p->next) {
                    last->next = merge(l, r);
                    break;
                }
                p->next = nullptr;
                p = r;
                for (int j = 0; j < size - 1; j++) {
                    if (p->next) p = p->next;
                    else break;
                }
                ListNode *next;
                if (p->next) next = p->next;
                p->next = nullptr;
                last->next = merge(l, r);
                while (last->next) last = last->next;
                cur = next;
            }
            head = dummy->next;
        }
        return head;
    }
};
```

## 用归并排序求数组中的逆序对数量

[LeetCode **剑指 Offer 51. 数组中的逆序对](https://leetcode.cn/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)          [AcWing 788](https://www.acwing.com/problem/content/790/)**

### [AcWing **788. 逆序对的数量**](https://www.acwing.com/problem/content/790/)

给定一个长度为 nn 的整数数列，请你计算数列中的逆序对的数量。

逆序对的定义如下：对于数列的第 ii 个和第 jj 个元素，如果满足 i<ji<j 且 a[i]>a[j]a[i]>a[j]，则其为一个逆序对；否则不是。

### **输入格式**

第一行包含整数 nn，表示数列的长度。

第二行包含 nn 个整数，表示整个数列。

### **输出格式**

输出一个整数，表示逆序对的个数。

### **数据范围**

1≤n≤1000001≤n≤100000，数列中的元素的取值范围 [1,109][1,109]。

### **输入样例：**

```
6
2 3 4 5 6 1

```

### **输出样例：**

```
5
```

```cpp
#include <bits/stdc++.h>

using namespace std;

using ll = long long;

const int N = 1e5 + 5;

int a[N], tmp[N];

ll merge_sort(int l, int r) {
    if (l >= r) return 0;
    int mid = (l + r) >> 1;
    long long res = merge_sort(l, mid) + merge_sort(mid + 1, r);
    int i = l, j = mid + 1, k = l;
    while (k <= r) {
        if (i > mid) tmp[k++] = a[j++];
        else if (j > r || a[i] <= a[j]) tmp[k++] = a[i++];
        else {
            tmp[k++] = a[j++];
            res += (ll) mid - i + 1;
        }
    }
    for (k = l; k <= r; k++) a[k] = tmp[k];
    return res;

}

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) cin >> a[i];
    cout << merge_sort(0, n - 1) << endl;
    return 0;
}
```