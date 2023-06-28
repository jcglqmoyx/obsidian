### [See Wikipedia](https://en.wikipedia.org/wiki/Quicksort)

Some sorting algorithms are stable by nature like **[Insertion sort](https://www.geeksforgeeks.org/insertion-sort/)**, **[Merge Sort](https://www.geeksforgeeks.org/merge-sort/)**, **[Bubble Sort](https://www.geeksforgeeks.org/bubble-sort/)**, etc. And some sorting algorithms are not, like **[Heap Sort](https://www.geeksforgeeks.org/heap-sort/)**, **[Quick Sort](https://www.geeksforgeeks.org/quick-sort/)**, etc. QuickSort is an unstable algorithm because we do swapping of elements according to pivot’s position (without considering their original positions).

Quicksort can be stable but it typically isn’t implemented that way. Making it stable either requires order N storage (as in a naive implementation) or a bit of extra logic for an in-place version.

---

Worst-case performance: $O(N^2)$

Best-case performance: $O(N*logN)$ or $O(N)$(three-way partition and equal keys)

Average performance: $O(N*logN)$

Space complexity $O(logN)$

---

## Quick-sort

[AcWing 785](https://www.acwing.com/problem/content/description/787/)       [LeetCode 912](https://leetcode.cn/problems/sort-an-array/description/)

```cpp
#include <bits/stdc++.h>

using namespace std;

int partition(vector<int> &nums, int low, int high) {
    int idx = low + rand() % (high - low + 1);
    swap(nums[low], nums[idx]);
    int pivot = nums[low];
    int i = low + 1, j = high;
    while (true) {
        while (i < high && nums[i] < pivot) i++;
        while (j > low && nums[j] > pivot) j--;
        if (i >= j) break;
        swap(nums[i], nums[j]);
        i++, j--;
    }
    swap(nums[low], nums[j]);
    return j;
}

void quicksort(vector<int> &nums, int low, int high) {
    if (low >= high) return;
    int p = partition(nums, low, high);
    quicksort(nums, low, p - 1);
    quicksort(nums, p + 1, high);
}

int main() {
    vector<int> nums = {6, 2, 8, 4, 3, 1, 9, 5, 7};
    quicksort(nums, 0, (int) nums.size() - 1);
}
```

## Tree-way quick-sort

```cpp
#include <bits/stdc++.h>

using namespace std;

void quicksort(vector<int> &arr, int low, int high) {
    if (low >= high) return;
    int idx = low + rand() % (high - low + 1);
    swap(arr[low], arr[idx]);
    int pivot = arr[low];
    int i = low + 1, lt = low, gt = high;
    while (i <= gt) {
        if (arr[i] < pivot) {
            swap(arr[lt++], arr[i++]);
        } else if (arr[i] > pivot) {
            swap(arr[i], arr[gt--]);
        } else {
            i++;
        }
    }
    quicksort(arr, low, lt - 1);
    quicksort(arr, gt + 1, high);
}

int main() {
    vector<int> arr = {5, 2, 7, 9, 6, 1, 8, 0, 4, 3};
    cout << "Before sorting: ";
    for (int n: arr) cout << n << " ";
    cout << endl << "After  sorting: ";
    quicksort(arr, 0, (int) arr.size() - 1);
    for (int n: arr) cout << n << " ";
    return 0;
}
```

## [Quick-select](https://en.wikipedia.org/wiki/Quickselect)

Best-case performance: $O(N)$

Worst-case performance: $O(N^2)$

Average performance: $O(N)$

[AcWing 786](https://www.acwing.com/problem/content/788/)      [LeetCode 215](https://leetcode.cn/problems/kth-largest-element-in-an-array/description/)

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1e5 + 5;

int n, k;
int a[N];

void quick_select(int low, int high, int k) {
    if (low >= high) return;
    int idx = low + rand() % (high - low + 1);
    swap(a[low], a[idx]);
    int pivot = a[low];
    int i = low + 1, j = high;
    while (true) {
        while (i < high && a[i] < pivot) i++;
        while (j > low && a[j] > pivot) j--;
        if (i >= j) break;
        swap(a[i], a[j]);
        i++, j--;
    }
    swap(a[low], a[j]);
    if (j < k - 1) quick_select(j + 1, high, k);
    else if (j > k - 1) quick_select(low, j - 1, k);
}

int main() {
    cin >> n >> k;
    for (int i = 0; i < n; i++) cin >> a[i];
    quick_select(0, n - 1, k);
    cout << a[k - 1] << endl;
    return 0;
}
```