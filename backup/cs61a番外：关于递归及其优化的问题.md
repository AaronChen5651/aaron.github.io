### **1.递归的一种优化方法：缓存优化（减少重复计算），以斐波那契数列为例**


**计算斐波那契数列**
Python 代码：
```python
def fibonacci(n):
    if n == 0:  # 终止条件
        return 0
    if n == 1:  # 终止条件
        return 1
    return fibonacci(n - 1) + fibonacci(n - 2)  # 递归调用
```
**递归过程示意**
```
fibonacci(5)
= fibonacci(4) + fibonacci(3)
= (fibonacci(3) + fibonacci(2)) + (fibonacci(2) + fibonacci(1))
= ((fibonacci(2) + fibonacci(1)) + fibonacci(2)) + (fibonacci(2) + 1)
= (((fibonacci(1) + fibonacci(0)) + fibonacci(1)) + fibonacci(2)) + (fibonacci(2) + 1)
= (((1 + 0) + 1) + fibonacci(2)) + (fibonacci(2) + 1)
= (((1 + 0) + 1) + ((1 + 0) + 1)) + (((1 + 0) + 1) + 1)
= 5
```
**缺点：重复计算！** 可以用 **缓存优化**。
**缓存优化（减少重复计算）**
**斐波那契数列** 的递归会**重复计算很多次**，可以使用 **缓存** 来优化：
```python
from functools import lru_cache

@lru_cache(None)  # 自动缓存计算结果
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

print(fibonacci(50))  # 只需几毫秒！
```
**优化效果：**
- 时间复杂度 **从 O(2ⁿ) 降到 O(n)**
- 计算 `fibonacci(50)` **快100万倍**！