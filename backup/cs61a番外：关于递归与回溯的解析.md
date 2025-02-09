示例：倒序打印
以 swipe 函数为例。目标是打印数字 n 的倒序，然后再按正序打印。我们用递归实现，回溯的过程就发生在递归结束后返回上一层时。

让我更清晰地解释一下 **回溯** 的过程和在递归中的作用。

### 回溯是如何工作的？

回溯实际上是指当递归函数达到基准条件并结束当前递归时，它会返回到上一层函数，继续执行上一层函数中递归调用之后的代码。这样，我们就完成了“**返回时执行**”的操作。

在你提到的代码中，回溯发生在递归调用到最深的地方后，开始逐层返回。

### 代码回顾：
```python
def swipe(n):
    if n < 10:
        print(n)  # 基本情况：数字小于 10 时打印数字
    else:
        print(n % 10)  # 打印当前数字的最后一位（倒序）
        swipe(n // 10)  # 递归调用，处理数字去掉最后一位的部分
        print(n % 10)  # 在回溯时打印当前数字的最后一位（正序）

swipe(234)
```

### 递归过程详细分析：

#### 第一步：调用 `swipe(234)`
- `234` 大于 `10`，进入 `else` 部分。
- `234 % 10` 结果是 `4`，打印 `4`（倒序打印）。
- 递归调用 `swipe(234 // 10)`，即 `swipe(23)`，继续进入递归。

#### 第二步：调用 `swipe(23)`
- `23` 大于 `10`，进入 `else` 部分。
- `23 % 10` 结果是 `3`，打印 `3`（倒序打印）。
- 递归调用 `swipe(23 // 10)`，即 `swipe(2)`，继续进入递归。

#### 第三步：调用 `swipe(2)`
- `2` 小于 `10`，进入 `if` 部分，打印 `2`（倒序打印）。
- 由于 `2` 小于 `10`，递归结束，不再进一步递归。

#### 回溯过程：
- 现在递归到达最深处（即 `swipe(2)`）后，开始返回到上一层函数（`swipe(23)`），这是递归的回溯过程。
- **回到 `swipe(23)`** 时，我们继续执行 `swipe(23)` 中递归调用之后的代码，即 `print(23 % 10)`，打印 `3`（正序打印）。
- 然后返回到 `swipe(234)`，继续执行 `swipe(234)` 中递归调用之后的代码，即 `print(234 % 10)`，打印 `4`（正序打印）。

### 输出结果：
```
4   # 倒序部分
3   # 倒序部分
2   # 倒序部分
3   # 正序部分（回溯时打印）
4   # 正序部分（回溯时打印）
```

### 为什么要回溯时再打印？
当递归调用进入更深层次时，它会先处理当前函数的递归部分（打印倒序）。当递归完成后，**返回到上一层函数时，递归调用已经结束**，此时我们需要**返回后打印数字**。这种方式保证了我们能在回溯时按照**正序**打印这些数字。

### 更简单的总结：
1. **递归**阶段：在每一层递归中，先打印当前层的最后一位数字（倒序）。
2. **回溯**阶段：当递归结束并返回上一层时，打印当前层的数字，完成正序的打印。

### 举个例子帮助理解：
假设你有 3 个数字：`234`，我们要先打印 `4 3 2`（倒序），然后再打印 `2 3 4`（正序）。
- 首先，递归会按顺序打印 `4 3 2`（倒序）。
- 然后在递归返回时，打印 `2 3 4`（正序）。

这样，递归就实现了倒序与正序打印的功能。

希望这次的解释更加清晰了！如果还有疑问，请随时告诉我。

---
### 下一个经典题目：自然数拆分


