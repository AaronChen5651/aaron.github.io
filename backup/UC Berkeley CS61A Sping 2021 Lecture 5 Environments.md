![image](https://github.com/user-attachments/assets/ae45ad1b-3437-43b2-909e-c2dd27b7116f)
永远要记得找父类，这个例子就很典型
f2找到g函数的时候就是往g的父类（也就是global）里面找f



![image](https://github.com/user-attachments/assets/565137da-cd4d-4c29-bc20-854d3d56b291)

def f(p, k):
    def g():
        print(k)
    if k== 0:
        f(g,1)
    else:
        p()

f(None, 0)
##print : 0
先进global，定义f
然后运行f(None, 0), 进f1(call的是f所以父环境是global),第一次定义一个g，这个g的父环境是f1
if，进入f2，f2是call的f所以父环境也是global（即使这个call行为是在f1 frame里干的，但是call的是f，所以parent environment是global），这时的p parameter定义为了g（上面那个父环境为f1的g），k parameter为1，然后运行到了def g，此时这个g是在f2里面的，和刚才的g不是一个g，最后运行else，运行p()，注意p定义的时f1里面的g，所以运行f1里的g，print（k），父类为f1，所以在f1里找k为0，所以打印0


![image](https://github.com/user-attachments/assets/1a270855-0b8b-41cc-a5cd-db928bca6b00)
比较简单，不讲


![image](https://github.com/user-attachments/assets/dee09a35-be90-4b34-873a-f035c6a1e0f2)
先定义f，然后在print(f(3))的时候进f1框架，调用f
f1开始，def g，然后运行g（4），运行就进f2，调用g，g里操作是使得x等于y，所以f2里x变成4，然后g函数什么也没返回，跳出，回到f1里继续运行
此时f1来到最后一行return x ，这个x是f1里面的，所以是3