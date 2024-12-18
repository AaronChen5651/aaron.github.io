### **Functions on Functions**

**两条原则**
1.函数应该只做一件明确的事（复杂的文档注释说明函数做得太多）
2.DRY(Don’t Repeat Yourself)，不要重复，出现重复的代码块建议重构（refactoring），将重复代码块替换成一个单独的函数
summation举例, 函数作为参数
函数是第一类值(first-class values)，可以作为变量传递给函数，也可以作为函数的返回值。

**根据这样的原则，尽量把函数（functions）变成模板（templates）！！**
![image](https://github.com/user-attachments/assets/8f00a9c1-1563-470b-9291-1700d756cfbf)

![image](https://github.com/user-attachments/assets/7871a0a9-1d17-484c-befd-4355b43377b5)
把k**2，替换成term(k)，形成模板


---
---

### **Functions that Produce Functions**

**高阶函数**的概念，重点是**函数可以返回另一个函数**。

---

### **1. 什么是函数的一等公民？**

在 Python 中，**函数是一等公民**（First-Class Values），这意味着：
1. **函数可以赋值给变量**。
2. **函数可以作为参数传递给另一个函数**。
3. **函数可以作为返回值** 返回给调用者。

简单的例子：
```python
def square(x):
    return x * x

f = square  # 把函数 square 赋值给变量 f
print(f(4))  # 调用 f，输出 16
```

---

### **2. 高阶函数 `add_func`**

#### **目标**：
我们要定义一个函数 `add_func`，它接收两个函数 `f` 和 `g`，返回一个新的函数 `adder`。这个新函数 `adder` 会把 `f(x)` 和 `g(x)` 的结果加起来。

---

#### **代码解析**：

```python
def add_func(f, g):
    """Return function that returns F(x) + G(x) for argument x."""
    def adder(x):  # 定义一个新的函数 adder
        return f(x) + g(x)  # 计算 f(x) 和 g(x) 的和
    return adder  # 返回 adder 函数
```

- `add_func(f, g)`：
   - 输入两个函数 `f` 和 `g`。
   - 返回一个新的函数 `adder`。
- `adder(x)`：
   - 接收一个参数 `x`。
   - 计算 `f(x)` 和 `g(x)`，然后把它们相加，返回结果。

---

#### **使用示例**：

```python
from math import sin, cos, pi

h = add_func(sin, cos)  # 调用 add_func，传入 sin 和 cos 函数
print(h(pi / 4))  # 调用返回的函数 h，传入 pi/4
```

- **`sin` 和 `cos` 是 Python 内置的数学函数**。
- `add_func(sin, cos)` 返回的函数 `h` 会计算 `sin(x) + cos(x)`。

**具体计算**：
- `x = pi / 4` 时，`sin(pi/4) ≈ 0.707`，`cos(pi/4) ≈ 0.707`。
- 所以 `sin(pi/4) + cos(pi/4) ≈ 1.414`。

**输出**：
```
1.414213562373095
```

---

### **总结这一页的重点**：

1. **函数可以返回另一个函数**。
2. **高阶函数** `add_func` 组合了两个函数的结果。
3. 你可以用 `add_func` 生成一个新的函数（比如 `h`），然后调用这个新函数。

---

**如何把函数组合逻辑泛化**，即让我们不仅能做加法，还能做其他操作，比如减法、乘法等。

---

### **1. 问题：如何更通用地组合两个函数？**

在上一页中，`add_func` 只能将两个函数的结果相加。但我们可以让它更加灵活，支持任意的操作，比如：
- 加法：`f(x) + g(x)`
- 乘法：`f(x) * g(x)`
- 最大值：`max(f(x), g(x))`

为此，我们定义一个**更通用的函数** `combine_funcs`。

---

### **2. `combine_funcs` 的代码**

```python
def combine_funcs(op):
    """General function-combining function."""
    def combined(f, g):  # 接收两个函数 f 和 g
        def val(x):  # 定义一个内部函数 val
            return op(f(x), g(x))  # 对 f(x) 和 g(x) 应用操作符 op
        return val  # 返回 val 函数
    return combined  # 返回 combined 函数
```

---

#### **代码解释**：

1. **`combine_funcs(op)`**：
   - `op` 是一个操作符，比如 `+`、`*`、`max` 等。
   - 返回一个函数 `combined`。

2. **`combined(f, g)`**：
   - 接收两个函数 `f` 和 `g`。
   - 返回一个函数 `val`。

3. **`val(x)`**：
   - 接收一个参数 `x`。
   - 对 `f(x)` 和 `g(x)` 应用操作符 `op`，并返回结果。

---

### **3. 构建 `add_func`**

我们可以通过 `combine_funcs` 来构建 `add_func`，使用 `operator.add` 作为操作符。

```python
from operator import add  # 导入加法操作符

add_func = combine_funcs(add)  # 使用 combine_funcs 创建 add_func
```

- `operator.add` 是 Python 内置的加法操作符。
- 现在 `add_func` 和上一页的 `add_func` 一样，功能是将两个函数的结果相加。

---

### **4. 使用示例**

```python
from math import sin, cos, pi

h = add_func(sin, cos)  # 使用 add_func 组合 sin 和 cos
print(h(pi / 4))  # 调用返回的函数 h，传入 pi/4
```

**输出**：
```
1.414213562373095
```

---

### **总结这一页的重点**：

1. **`combine_funcs`** 是一个通用的函数组合工具，可以通过操作符 `op` 组合两个函数的结果。
2. **`operator.add`** 被用来构建 `add_func`。
3. **函数的组合变得更加灵活**，不仅可以加法，还可以通过不同的操作符做其他操作。

---

## **整体总结**

1. **第一页**：展示了如何用函数返回另一个函数，组合两个函数的输出（加法）。
2. **第二页**：将函数组合逻辑泛化，支持任意操作符，而不仅仅是加法。

通过这样的代码设计，你可以构建灵活的函数组合工具，充分体现了**函数式编程**的思想。