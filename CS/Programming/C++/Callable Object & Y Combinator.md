```cpp
#include <bits/stdc++.h>  
  
using namespace std;  
  
class Solution {  
public:  
    vector<vector<int>> subsets(vector<int> &nums) {  
        vector<int> path;  
        vector<vector<int>> subsets;
          
        function<void(int)> dfs = [&](int u) {  
            if (u == nums.size()) {  
                subsets.emplace_back(path);  
            } else {  
                dfs(u + 1);  
  
                path.emplace_back(nums[u]);  
                dfs(u + 1);  
                path.pop_back();  
            }  
        };  
        
        dfs(0);
        
        return subsets;  
    }  
};
```


```cpp
        auto dfs2 = [&](auto &&dfs2, int u, int p) -> void { // if there's not a return value, you must explicitly write void here
            if (p != -1) {
                up[u] = up[p];
                if (idx1[p] == u) up[u] = max(up[u], down2[p]);
                else up[u] = max(up[u], down1[p]);
                up[u] += 1 + !(p & 1);
            }
            res[u] = max(down1[u], up[u]);
            for (int i = h[u]; ~i; i = ne[i]) {
                int j = e[i];
                if (j == p) continue;
                dfs2(dfs2, j, u);
            }
        };
        dfs2(dfs2, 0, -1);
```