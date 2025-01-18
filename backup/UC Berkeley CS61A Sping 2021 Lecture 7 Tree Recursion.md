树形递归

**树形递归（Tree Recursion）** 是指递归调用不仅仅是调用自身一次，而是调用自己多个次数，从而形成一个类似树状的结构。在树形递归中，每个递归调用可以生成多个子问题，这些子问题进一步递归调用自己，最终产生一个树状的递归调用结构。

### 树形递归的特点：
1. **多个子问题**：每个递归调用不仅会调用一次自身，还可能会产生多个递归调用，形成“树”的分支。
2. **递归深度增加**：每次递归调用都可能生成多个子递归调用，因此递归的深度比线性递归要大。

### 例子：斐波那契数列（Fibonacci Sequence）

```python
def fibonacci(n):
    if n <= 1:
        return n
    else:
        return fibonacci(n - 1) + fibonacci(n - 2)
```

- **解释**：
  - 对于 `fibonacci(5)`，它将计算 `fibonacci(4)` 和 `fibonacci(3)`，然后再分别计算这两个值。
  - 每次 `fibonacci` 函数调用会继续调用两个子问题（`fibonacci(n-1)` 和 `fibonacci(n-2)`），从而形成一棵递归调用的“树”。
  
  递归树如下：
          fibonacci(5)
         /          \
  fibonacci(4)    fibonacci(3)
     /      \         /      \
fibonacci(3) fibonacci(2) fibonacci(2) fibonacci(1)
  /    \        /      \       /     \
fibonacci(2) fibonacci(1) fibonacci(1) fibonacci(0)


### 树形递归的计算成本：
- 在树形递归中，许多子问题会被重复计算。以斐波那契数列为例，`fibonacci(3)` 被调用了两次，而 `fibonacci(2)` 被调用了三次。这样会导致冗余的计算。
- 可以通过**记忆化**（memoization）或**动态规划**（dynamic programming）来优化树形递归，避免重复计算。

### 其他树形递归的例子：
1. **汉诺塔问题（Tower of Hanoi）**：
   - 每次递归调用都会把问题分成多个子问题，因此可以看作是树形递归。

2. **排列与组合（Permutations and Combinations）**：
   - 求排列或组合问题时，递归会分别计算多个选择，从而形成树状结构。

### 总结：
树形递归的特征是每个递归调用都会生成多个子问题，这些子问题会再次递归调用，最终形成一个递归树结构。它可以用来解决许多复杂的问题，但也可能带来重复计算的问题，需要优化以提高效率。