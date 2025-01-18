![Image](https://github.com/user-attachments/assets/81cc75a8-4b1a-4ffe-9447-7f8526951a8b)

print_sums(1)(3)(5)应该理解为先运行print_sums(1)，然后运行print_sums(1)的(3)，然后运行print_sums(1)(3)的(5)。

运行步骤f1：n=1，print出来一个1，然后定义了一个新的函数next_sum(n+k)，最后return这个function next_sum(k)，于是我们立马就得到一个新的框架f2，在这里面call了next_sum(k)，并且k是3。

f2的parent是f1，因为f1call的是print_sums(n)，f2call的是next_sum(k) ，在代码中，print_sums(n) 是主函数，而 next_sum(k) 是在 print_sums(n) 函数内部定义的。当你调用 next_sum(3) 时，它在 print_sums 函数内部作为局部函数被执行，所以会创建一个新的框架 f2，这个框架是为 next_sum(k) 函数分配的。

call到next_sum了于是来到第三行，（n+k）其中的n没有在f2里定义，但是在其parent的f1里面定义了，所以下一行会是print_sums(4)，于是运行第四行，是个return，调用的是print_sums函数，注意，parent是根据你调用的这个函数来走的，所以f3的parent是global（根据print_sums来判定的）。

调用print_sums，于是来到第一行，运行print_sums(4)，第二行print(4)输出4，第三行定义next_sum，注意此时next_sum是在f3作为parent的环境里面定义的，里面参数要用 f3的了，感觉这一步就是递归的精髓。于是就是在f4框架运行 next_sum(5)，5是第七行那个5，来到第三行，在第四行创建框架f5运行print_sum(4+5)，来到第一行，最后print(9)。最后的返回值其实是fun next_sum(k) [parent = f5]，但没用上而已。

PPT上的解释：

![Image](https://github.com/user-attachments/assets/5524f38c-b1d0-4de5-be50-43396c90053b)

---
下一个话题，柯里化函数（currying）如果要深入研究就自行搜这个词

![Image](https://github.com/user-attachments/assets/3ebb2da0-1cb7-4bc2-86ef-6ca2b6caf6ee)
这段代码展示了如何使用“柯里化”（currying）将一个接受两个参数的函数转化为一个接受一个参数并返回另一个接受一个参数的函数的形式。

### 解释：

1. **柯里化函数（`curry2`）**：
   - `curry2(f)` 是一个函数，它接受一个函数 `f`，并返回一个新的函数。
   - 这个返回的函数接受一个参数 `x`，并且返回另一个函数，这个函数接受参数 `y`。然后，它使用原始的函数 `f(x, y)` 计算结果。

2. **Lambda 表达式**：
   - `lambda x: lambda y: f(x, y)` 是一个嵌套的 lambda 表达式，它的作用是将 `f` 的两个参数分开处理。
   - `lambda x` 表示接受第一个参数 `x`，然后返回一个新的函数 `lambda y`，该函数接受第二个参数 `y`。然后，计算 `f(x, y)`。

3. **代码示例**：
   - `from operator import add` 导入 Python 的 `add` 函数，它是一个简单的加法函数。
   - `curry2(add)` 将 `add` 函数进行柯里化处理，使其变成接受一个参数的函数。
   - `curry2(add)(30)` 返回一个新的函数，这个新函数接受参数 `y`，并计算 `add(30, y)`。
   - `curry2(add)(30)(12)` 最终计算的是 `add(30, 12)`，即 `30 + 12`。

### 输出解释：
- 第一行 `print(curry2(add)(30)(12))` 输出 `42`，因为它等价于计算 `add(30, 12)`。
- 第二行 `print(curry2(add)(30))` 返回的是一个函数，打印的是这个函数本身，而不是计算结果。

通过柯里化，你可以将多参数函数转化为一系列只接受一个参数的函数。这种方式使得你可以部分应用函数（即提前给定部分参数）。

第二行的代码是：

```python
print(curry2(add)(30))
```

这个打印的结果是一个 **函数对象**，而不是一个具体的值。原因如下：

- `curry2(add)` 将 `add` 函数进行了柯里化处理，返回一个接受参数 `x` 的新函数。
- 当你执行 `curry2(add)(30)` 时，它会返回一个新的函数，该函数仍然等待一个参数 `y`。
- 所以，第二行代码实际上打印的是返回的这个新的函数对象，而不是计算结果。

因此，第二行的输出是类似于：

```python
<function <lambda> at 0x...>
```

它表示一个 lambda 函数的内存地址。
---
为了看得清楚，他把lambda x和lambda y写到两行去，下面是只运行第一个print的时候的情况

![Image](https://github.com/user-attachments/assets/41dafbbf-8b55-4185-8fb8-76b131948add)

下面是运行第二个print的情况

![Image](https://github.com/user-attachments/assets/1e63dd33-8849-4a21-9327-744145cefa5b)


---

下一个话题，函数的哲学和规范

![Image](https://github.com/user-attachments/assets/04892cf8-be79-42fb-b74f-b35df436814e)

这张图展示了函数的哲学和规范，重点讲解了 **语法规范（Syntactic specification）** 和 **语义规范（Semantic specification）** 的概念。具体解释如下：

### 1. **语法规范（Syntactic specification）**
   - 语法规范描述了如何调用函数（例如参数的数量和类型）。这包括函数的**签名**，也就是定义函数时指定的名称和参数。
   - 在代码中，`def sqrt(x):` 就是一个函数签名，它告诉我们该函数的名称是 `sqrt`，并且它接受一个参数 `x`。

### 2. **语义规范（Semantic specification）**
   - 语义规范告诉我们函数的实际功能或行为，明确了函数的**预条件（Preconditions）**和**后条件（Postconditions）**。
     - **预条件（Precondition）**：是函数执行前必须满足的要求。在这个例子中，预条件是 `Assuming X >= 0`，表示输入的 `x` 必须大于等于 0。
     - **后条件（Postcondition）**：是函数执行后承诺的结果或保证。在这个例子中，后条件是 `returns approximation to square root of X`，即函数将返回 `x` 的平方根的近似值。

### 3. **合同（Contract）**
   - 语法和语义规范共同定义了调用者与函数实现者之间的“合同”。
   - 合同指定了函数的使用规则，即调用者需要提供什么样的输入（预条件），以及函数会返回什么样的输出（后条件）。

### 总结：
- **语法规范** 描述了如何正确地调用函数（参数的数量和类型）。
- **语义规范** 描述了函数的行为，具体来说，就是函数应该如何处理输入并返回什么样的结果。
- 预条件和后条件共同定义了调用函数时的要求和函数执行后应满足的承诺。


---

简单线性递归

![Image](https://github.com/user-attachments/assets/f8e54fe2-3855-4ee4-ada6-84bd13d4fe4d)

这张图展示了**简单线性递归**的概念，并通过一个示例函数 `sum_squares(N)` 来演示。以下是详细解释：

### 1. **递归函数 `sum_squares(N)`**：
   - 该函数的目标是计算从 1 到 `N`（包括 `N`）的所有整数的平方和。
   - 函数的定义如下：
     ```python
     def sum_squares(N):
         if N < 1:
             return 0
         else:
             return sum_squares(N - 1) + N**2
     ```
     - **递归基准条件（Base case）**：当 `N < 1` 时，函数返回 0。这个条件停止递归过程，防止无限递归。
     - **递归步骤**：如果 `N >= 1`，函数调用自身 `sum_squares(N - 1)`，并将当前数字 `N` 的平方加到结果中。

### 2. **递归展开示例**：
   - 假设我们要计算 `sum_squares(3)`，函数的执行过程会展开如下：
     - `sum_squares(3)` 等于 `sum_squares(2) + 3^2`，即 `sum_squares(2) + 9`
     - `sum_squares(2)` 等于 `sum_squares(1) + 2^2`，即 `sum_squares(1) + 4`
     - `sum_squares(1)` 等于 `sum_squares(0) + 1^2`，即 `sum_squares(0) + 1`
     - `sum_squares(0)` 返回 0（这是递归基准条件）。
     - 所以，最终计算结果为：`0 + 1^2 + 2^2 + 3^2 = 14`

### 3. **递归的本质**：
   - 递归意味着一个函数在其定义过程中调用自己（直接或间接）。
   - 在上面的例子中，`sum_squares` 函数直接调用自身，以处理较小的 `N` 值，直到达到基准条件为止。

### 4. **环境框架**：
   - 每一次递归调用都会生成一个新的环境框架（环境变量的上下文）。每个递归调用都有自己的函数实例，并且栈中的每个框架都会保存当前的 `N` 值和返回值。
   - 这些框架（在图中的“Python Tutor”示例中）会彼此链接，形成递归调用的链条。

### 总结：
- **简单线性递归** 是一种递归方式，其中每个函数调用都只会调用一次自己。这个过程直到满足基准条件停止递归。
- 递归的关键是通过重复的自我调用来逐步减少问题的规模，最终到达一个已知的简单情况（基准条件）。

---
尾递归 tail recurtion

![Image](https://github.com/user-attachments/assets/c2d22b33-cb0f-4ede-be9b-542b8e9a9176)


这张图展示了**尾递归（Tail Recursion）**的概念，并通过一个平方和的例子进行了比较。以下是对图中内容的解释：

1. 尾递归（Tail Recursion）：
尾递归是递归的一种特殊形式，其中递归调用是函数的最后一步，即递归调用是函数的返回值，或者是递归调用之后没有更多的操作。
在尾递归中，递归调用不需要保持函数调用栈上的额外信息，因为递归调用的返回值就是最终结果，这使得尾递归比普通递归更高效。

2. 两种实现方式：
左边的版本：使用 while 循环实现平方和的计算。
右边的版本：使用尾递归实现平方和的计算，带有 part_sum 函数。

这段代码展示了两种不同的实现方式来计算整数的平方和。我们可以逐行理解这段代码：

### 第一部分：使用 `while` 循环实现的平方和

```python
def sum_squares(N):
    """The sum of K**2 for 1 <= K <= N."""
    accum = 0
    k = 1
    while k <= N:
        accum = accum + k**2
        k = k + 1
        print("accum =", accum, "/ k =", k)
    return accum
print(sum_squares(3))
```

#### 1. `sum_squares(N)` 函数：
   - **功能**：该函数计算从 1 到 `N` 之间所有整数的平方和。
   - **参数**：`N` 是计算的上限。
   
#### 2. 代码分析：
   - **初始化**：
     - `accum = 0`：用来累加平方和的变量。
     - `k = 1`：`k` 用来循环遍历从 1 到 `N` 的整数。
   - **`while k <= N`**：这是一个循环，条件是当 `k` 小于等于 `N` 时继续循环。
     - 在每次循环中：
       - `accum = accum + k**2`：将 `k` 的平方加到 `accum` 中。
       - `k = k + 1`：递增 `k`，确保循环继续。
       - `print("accum =", accum, "/ k =", k)`：打印当前的累加值和 `k`。
   - **返回值**：当 `k` 超过 `N` 时，退出循环，返回最终的 `accum` 值。

#### 3. `sum_squares(3)` 执行：
   - 对于 `N = 3`，程序执行时将计算 `1^2 + 2^2 + 3^2`。
   - 输出：
     ```
     accum = 1 / k = 2
     accum = 5 / k = 3
     accum = 14 / k = 4
     ```
     最终结果是 `14`，即 `1^2 + 2^2 + 3^2 = 14`。

### 第二部分：递归实现的平方和

```python
def sum_squares(N):
    def part_sum(accum, k):
        if k <= N:
            return part_sum(accum + k**2, k + 1)
        else:
            return accum
    return part_sum(0, 1)
print(sum_squares(3))
```

#### 1. `sum_squares(N)` 函数：
   - **功能**：与前一个函数相同，计算从 1 到 `N` 之间所有整数的平方和。
   
#### 2. 代码分析：
   - **递归方式**：这个实现方式使用了递归来计算平方和。
   - **`part_sum(accum, k)`**：
     - `accum` 是累积平方和的变量。
     - `k` 是当前处理的整数。
   - **递归步骤**：
     - 如果 `k <= N`，则递归调用 `part_sum(accum + k**2, k + 1)`，即将 `k` 的平方加到 `accum` 中，然后处理下一个整数 `k + 1`。
     - 如果 `k > N`，则返回当前的 `accum`，结束递归。
   - **返回值**：最终通过 `part_sum(0, 1)` 开始递归，累加所有整数的平方和。

#### 3. `sum_squares(3)` 执行：
   - 对于 `N = 3`，递归过程如下：
     - 第一次调用 `part_sum(0, 1)`，计算 `0 + 1^2`，递归到 `part_sum(1, 2)`。
     - 第二次调用 `part_sum(1, 2)`，计算 `1 + 2^2`，递归到 `part_sum(5, 3)`。
     - 第三次调用 `part_sum(5, 3)`，计算 `5 + 3^2`，递归到 `part_sum(14, 4)`。
     - 最后调用 `part_sum(14, 4)`，由于 `k > 3`，返回 `14`。
   
   最终返回值为 `14`，即 `1^2 + 2^2 + 3^2 = 14`。

### 总结：
- 第一部分使用了 `while` 循环来迭代计算平方和，打印每一步的累加值。
- 第二部分使用递归来计算平方和，每次递归时计算当前数字的平方并将其加到累积和中。

4. 尾递归的特点：
右边的递归实现是尾递归，因为递归调用是函数的最后一步，返回的就是递归调用的结果。
尾递归的好处是它可以被编译器或解释器优化为迭代过程，从而减少栈空间的使用，避免栈溢出。
5. 总结：
尾递归与普通递归的主要区别在于递归调用是否是最后执行的操作。在尾递归中，递归调用不需要保留函数调用栈中的额外信息，因此可以进行优化，减少内存消耗。
该技巧也可以应用到 while 循环中，将其转化为递归形式。