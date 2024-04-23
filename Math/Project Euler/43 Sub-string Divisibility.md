---
page-title: 43 Sub-string Divisibility
url: https://projecteuler.net/problem=43
date: 2024-04-23 20:51:57
---
---
The number, $1406357289$, is a $0$ to $9$ pandigital number because it is made up of each of the digits $0$ to $9$ in some order, but it also has a rather interesting sub-string divisibility property.
Let $d_1$ be the $1$<sup>st</sup> digit, $d_2$ be the $2$<sup>nd</sup> digit, and so on. In this way, we note the following:

$d_2d_3d_4=406$ is divisible by $2$
$d_3d_4d_5=063$ is divisible by $3$
$d_4d_5d_6=635$ is divisible by $5$
$d_5d_6d_7=357$ is divisible by $7$
$d_6d_7d_8=572$ is divisible by $11$
$d_7d_8d_9=728$ is divisible by $13$
$d_8d_9d_{10}=289$ is divisible by $17$

Find the sum of all $0$ to $9$ pandigital numbers with this property.

Answer: 16695334890

```python
digits = [x for x in range(0, 10)]  
res = 0  
prime = [-1, 2, 3, 5, 7, 11, 13, 17]  
  
  
def valid(x):  
    s = str(x)  
    for i in range(1, len(prime)):  
        if int(s[i:i + 3]) % prime[i] != 0:  
            return False  
    return True  
  
def dfs(u, x, mask):  
    global res  
    if u == len(digits):  
        if valid(x):  
            res += x  
    else:  
        for i in range(len(digits)):  
            if mask >> i & 1:  
                continue  
            mask ^= 1 << i  
            dfs(u + 1, x * 10 + digits[i], mask)  
            mask ^= 1 << i  
  
  
dfs(0, 0, 0)  
print(res)
```