[Codeforces 197 A. Plate Game](https://codeforces.com/contest/197/problem/A)

[Codeforces D. Little Girl and Maximum XOR](https://codeforces.com/contest/276/problem/D)

[Codeforces 584 C. Marina and Vasya](https://codeforces.com/contest/584/problem/C)

[Codeforces 830 A. Office Keys](https://codeforces.com/problemset/problem/830/A)

[Codeforces 1396 A. Multiples of Length](https://codeforces.com/contest/1396/problem/A)

## [LeetCode 1041. Robot Bounded In Circle](https://leetcode.cn/problems/robot-bounded-in-circle/description/)
> On an infinite plane, a robot initially stands at `(0, 0)` and faces north. Note that:
> 
> -   The **north direction** is the positive direction of the y-axis.
> -   The **south direction** is the negative direction of the y-axis.
> -   The **east direction** is the positive direction of the x-axis.
> -   The **west direction** is the negative direction of the x-axis.
> 
> The robot can receive one of three instructions:
> 
> -   `"G"`: go straight 1 unit.
> -   `"L"`: turn 90 degrees to the left (i.e., anti-clockwise direction).
> -   `"R"`: turn 90 degrees to the right (i.e., clockwise direction).
> 
> The robot performs the `instructions` given in order, and repeats them forever.
> 
> Return `true` if and only if there exists a circle in the plane such that the robot never leaves the circle.
> 
> **Example 1:**
> 
> **Input:** instructions = "GGLLGG"
> **Output:** true
> **Explanation:** The robot is initially at (0, 0) facing the north direction.
> "G": move one step. Position: (0, 1). Direction: North.
> "G": move one step. Position: (0, 2). Direction: North.
> "L": turn 90 degrees anti-clockwise. Position: (0, 2). Direction: West.
> "L": turn 90 degrees anti-clockwise. Position: (0, 2). Direction: South.
> "G": move one step. Position: (0, 1). Direction: South.
> "G": move one step. Position: (0, 0). Direction: South.
> Repeating the instructions, the robot goes into the cycle: (0, 0) --> (0, 1) --> (0, 2) --> (0, 1) --> (0, 0).
> Based on that, we return true.
> 
> **Example 2:**
> 
> **Input:** instructions = "GG"
> **Output:** false
> **Explanation:** The robot is initially at (0, 0) facing the north direction.
> "G": move one step. Position: (0, 1). Direction: North.
> "G": move one step. Position: (0, 2). Direction: North.
> Repeating the instructions, keeps advancing in the north direction and does not go into cycles.
> Based on that, we return false.
> 
> **Example 3:**
> 
> **Input:** instructions = "GL"
> **Output:** true
> **Explanation:** The robot is initially at (0, 0) facing the north direction.
> "G": move one step. Position: (0, 1). Direction: North.
> "L": turn 90 degrees anti-clockwise. Position: (0, 1). Direction: West.
> "G": move one step. Position: (-1, 1). Direction: West.
> "L": turn 90 degrees anti-clockwise. Position: (-1, 1). Direction: South.
> "G": move one step. Position: (-1, 0). Direction: South.
> "L": turn 90 degrees anti-clockwise. Position: (-1, 0). Direction: East.
> "G": move one step. Position: (0, 0). Direction: East.
> "L": turn 90 degrees anti-clockwise. Position: (0, 0). Direction: North.
> Repeating the instructions, the robot goes into the cycle: (0, 0) --> (0, 1) --> (-1, 1) --> (-1, 0) --> (0, 0).
> Based on that, we return true.
> 
> **Constraints:**
> 
> -   `1 <= instructions.length <= 100`
> -   `instructions[i]` is `'G'`, `'L'` or, `'R'`.

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    bool isRobotBounded(string instructions) {  
        int dx[4] = {0, 1, 0, -1}, dy[4] = {1, 0, -1, 0};  
        int d = 0;  
        int x = 0, y = 0;  
        for (int i = 0; i < 4; i++) {  
            for (char c: instructions) {  
                if (c == 'G') {  
                    x += dx[d], y += dy[d];  
                } else if (c == 'L') {  
                    d = (d + 3) % 4;  
                } else if (c == 'R') {  
                    d = (d + 1) % 4;  
                }  
            }  
        }  
        return x == 0 && y == 0 || d;  
    }  
    
};
```



## [LeetCode 2551. Put Marbles in Bags](https://leetcode.cn/problems/put-marbles-in-bags/description/)

## [LeetCode 1734. Decode XORed Permutation](https://leetcode.cn/problems/decode-xored-permutation/description/)

## [LeetCode 2306. Naming a Company](https://leetcode.cn/problems/naming-a-company/description/)

## [LeetCode 2444. Count Subarrays With Fixed Bounds](https://leetcode.cn/problems/count-subarrays-with-fixed-bounds/description/)

## [LeetCode 2556. Disconnect Path in a Binary Matrix by at Most One Flip](https://leetcode.cn/problems/disconnect-path-in-a-binary-matrix-by-at-most-one-flip/)

## [LeetCode 2565. Subsequence With the Minimum Score](https://leetcode.cn/problems/subsequence-with-the-minimum-score/description/)


## [ARC 119C](https://atcoder.jp/contests/arc119/tasks/arc119_c)
There are N buildings along the AtCoder Street, numbered 1 through N from west to east. Initially, Buildings 1,2,…,N have the heights of $A_1, A_2,...,A_N$respectively.

Takahashi, the president of ARC Wrecker, Inc., plans to choose integers l and r (1≤l<r≤N) and make the heights of Buildings l,l+1,…,r all zero. To do so, he can use the following two kinds of operations any number of times in any order:

-   Set an integer x (l≤x≤r−1) and increase the heights of Buildings xx and x+1 by 1 each.
-   Set an integer x (l≤x≤r−1) and decrease the heights of Buildings xx and x+1 by 1 each. This operation can only be done when both of those buildings have heights of 1 or greater.

Note that the range of xx depends on (l,r).

How many choices of (l,r) are there where Takahashi can realize his plan?

### Constraints

-   2≤N≤300000
-   1≤$A_i$≤$10^9$ ($1\le i \le N$)
-   All values in input are integers.

---

### Input

Input is given from Standard Input in the following format:

N
$A_1, A_2,  ... , A_N$

---

### Sample Input 1 Copy
```
5
5 8 8 6 6
```


### Sample Output 1 Copy
```
3
```

Takahashi can realize his plan for (l,r)\=(2,3),(4,5),(2,5).

For example, for (l,r) = (2,5), the following sequence of operations make the heights of Buildings 2, 3, 4, 5 all zero.

-   Decrease the heights of Buildings 4 and 5 by 1 each, six times in a row.
-   Decrease the heights of Buildings 2 and 3 by 1 each, eight times in a row.

For the remaining seven choices of (l, r), there is no sequence of operations that can realize his plan.

---

### Sample Input 2 Copy

```
7
12 8 11 3 3 13 2
```

### Sample Output 2 Copy

```
3
```

Takahashi can realize his plan for (l, r) = (2, 4), (3, 7), (4, 5).

For example, for (l, r) = (3, 7), the following figure shows one possible solution.

![ ](https://img.atcoder.jp/arc119/392b686a479008a3dbc3fb36893ed144.png)

---

### Sample Input 3 Copy
```
10
8 6 3 9 5 4 7 2 1 10
```

### Sample Output 3 Copy

```
1
```

Takahashi can realize his plan for (l, r) = (3, 8) only.

---

### Sample Input 4 Copy

```
14
630551244 683685976 249199599 863395255 667330388 617766025 564631293 614195656 944865979 277535591 390222868 527065404 136842536 971731491
```

### Sample Output 4 Copy

```
8
```
---

```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
int main() {  
    int n;  
    scanf("%d", &n);  
    long long res = 0, sum = 0;  
    unordered_map<long long, int> map;  
    map[0] = 1;  
    while (n--) {  
        int x;  
        scanf("%d", &x);  
        sum += n & 1 ? x : -x;  
        res += map[sum];  
        map[sum]++;  
    }  
    printf("%lld\n", res);  
    return 0;  
}
```