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