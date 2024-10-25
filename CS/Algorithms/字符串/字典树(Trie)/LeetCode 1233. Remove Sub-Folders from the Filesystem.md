---
page-title: 1233. Remove Sub-Folders from the Filesystem
url: https://leetcode.com/problems/remove-sub-folders-from-the-filesystem/description/?envType=daily-question&envId=2024-10-25
date: 2024-10-25 11:42:22
---
Given a list of folders `folder`, return *the folders after removing all **sub-folders** in those folders*. You may return the answer in **any order**.

If a `folder[i]` is located within another `folder[j]`, it is called a **sub-folder** of it. A sub-folder of `folder[j]` must start with `folder[j]`, followed by a `"/"`. For example, `"/a/b"` is a sub-folder of `"/a"`, but `"/b"` is not a sub-folder of `"/a/b/c"`.

The format of a path is one or more concatenated strings of the form: `'/'` followed by one or more lowercase English letters.

-   For example, `"/leetcode"` and `"/leetcode/problems"` are valid paths while an empty string and `"/"` are not.

**Example 1:**

**Input:** folder = \["/a","/a/b","/c/d","/c/d/e","/c/f"\]
**Output:** \["/a","/c/d","/c/f"\]
**Explanation:** Folders "/a/b" is a subfolder of "/a" and "/c/d/e" is inside of folder "/c/d" in our filesystem.

**Example 2:**

**Input:** folder = \["/a","/a/b/c","/a/b/d"\]
**Output:** \["/a"\]
**Explanation:** Folders "/a/b/c" and "/a/b/d" will be removed because they are subfolders of "/a".

**Example 3:**

**Input:** folder = \["/a/b/c","/a/b/ca","/a/b/d"\]
**Output:** \["/a/b/c","/a/b/ca","/a/b/d"\]

**Constraints:**

-   `1 <= folder.length <= 4 * 104`
-   `2 <= folder[i].length <= 100`
-   `folder[i]` contains only lowercase letters and `'/'`.
-   `folder[i]` always starts with the character `'/'`.
-   Each folder name is **unique**.

```cpp
#include <bits/stdc++.h>  
  
#include <utility>  
  
using namespace std;  
  
struct Node {  
    string val;  
    bool is_end;  
    unordered_map<string, Node *> ne;  
  
    explicit Node(string _val) {  
        this->val = std::move(_val);  
        this->is_end = false;  
    }  
};  
  
class Solution {  
public:  
    vector<string> removeSubfolders(vector<string> &folder) {  
        sort(folder.begin(), folder.end());  
        auto root = new Node("/");  
        vector<string> res;  
        for (auto &f: folder) {  
            auto p = root;  
            bool valid = true;  
            for (int prev = 0, i = 1; i < f.size(); i++) {  
                if (i < f.size() && f[i + 1] == '/' || i == f.size() - 1) {  
                    string s = f.substr(prev, i - prev + 1);  
                    if (p->ne.contains(s)) {  
                        if (p->ne[s]->is_end) {  
                            valid = false;  
                            break;  
                        }  
                    } else {  
                        p->ne[s] = new Node(s);  
                    }  
                    p = p->ne[s];  
                }  
            }  
            if (valid) {  
                p->is_end = true;  
                res.emplace_back(f);  
            }  
        }  
        delete root;  
        return res;  
    }  
};
```