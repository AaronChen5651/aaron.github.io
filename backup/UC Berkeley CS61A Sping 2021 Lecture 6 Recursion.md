![Image](https://github.com/user-attachments/assets/81cc75a8-4b1a-4ffe-9447-7f8526951a8b)

print_sums(1)(3)(5)应该理解为先运行print_sums(1)，然后运行print_sums(1)的(3)，然后运行print_sums(1)(3)的(5)。

运行步骤f1：n=1，print出来一个1，然后定义了一个新的函数next_sum(n+k)，最后return这个function next_sum(k)，于是我们立马就得到一个新的框架f2，在这里面call了next_sum(k)，并且k是3。

f2的parent是f1，因为f1call的是print_sums(n)，f2call的是next_sum(k) ，在代码中，print_sums(n) 是主函数，而 next_sum(k) 是在 print_sums(n) 函数内部定义的。当你调用 next_sum(3) 时，它在 print_sums 函数内部作为局部函数被执行，所以会创建一个新的框架 f2，这个框架是为 next_sum(k) 函数分配的。

call到next_sum了于是来到第三行，（n+k）其中的n没有在f2里定义，但是在其parent的f1里面定义了，所以下一行会是print_sums(4)，于是运行第四行，是个return，调用的是print_sums函数，注意，parent是根据你调用的这个函数来走的，所以f3的parent是global（根据print_sums来判定的）。

调用print_sums，于是来到第一行，运行print_sums(4)，第二行print(4)输出4，第三行定义next_sum，注意此时next_sum是在f3作为parent的环境里面定义的，里面参数要用 f3的了，感觉这一步就是递归的精髓。于是就是在f4框架运行 next_sum(5)，5是第七行那个5，来到第三行，在第四行创建框架f5运行print_sum(4+5)，来到第一行，最后print(9)。最后的返回值其实是fun next_sum(k) [parent = f5]，但没用上而已。