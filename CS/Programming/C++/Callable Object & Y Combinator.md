`std::function` 是一个通用的可调用对象的包装器，可以持有任何符合特定函数签名的可调用对象（如函数指针、lambda、绑定的函数等）。当你使用 `std::function` 时，返回类型已经由 `std::function` 的模板参数指定。
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

在C++中如果使用Y组合子定义lambda表达式并且不返回任何值，最好显式地指定返回类型为`void`。如果不显式指明，编译器可能会因为无法推断出返回类型而报错。
```cpp
        auto dfs2 = [&](auto &&dfs2, int u, int p) -> void {
        // 使用Y组合因子定义lambda表达式，显式地指明返回类型为 void
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