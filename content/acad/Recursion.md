---
title: "Recursion"
date: 2021-01-17T19:31:28+08:00
draft: false
dropCap: false
tags: [编程, 递归]
---

### 相关资源

- [自学计算机科学](https://github.com/keithnull/TeachYourselfCS-CN/blob/master/TeachYourselfCS-CN.md)
- [伯克利大学编程课(python)](https://inst.eecs.berkeley.edu/~cs61a/fa20/)

### 递归

[Luhn算法](https://zh.wikipedia.org/zh-hans/Luhn%E7%AE%97%E6%B3%95)是一种简单的校验和算法，方式是对一串数字，从校验位开始从右往左，偶数位乘2，奇数位不变，两位数字的个位与十位相加，最后把所有得到的数字相加即为校验和。

此问题的简化版本为：

> 给一个数字，求其所有数字之和。

可以用递归的方法解决。

首先是将数字拆分为除最后位的其他数字与最后一位的算法：

``` python
def split(n):
    return n // 10, n % 10
```

然后是递归算法主体：

``` python
def sum_digits(n):
    if n < 10:  # 简单形式
        return n
    else:
        all_but_last, last = split(n)
        return sum_digits(all_but_last) + last  # 递归调用
```

``` bash
>>> split(432)
>>> (43, 2)
>>> sum_digits(432)
>>> 9
```

#### Question: cascade

需要用例子来体会递归的调用顺序。

``` python
def cascade(n):
    if n < 10:
        print(n)
    else:
        print(n)
        cascade(n//10)
        print(n)
```

``` bash
>>> cascade(12345)
12345
1234
123
12
1
12
123
1234
12345
```

注意，第一个12345是 else 里的第一个 print 打印的，最后一个12345是 else 里的第二个 print 打印的（注意要等到递归的 cascade 函数返回后才打印），中间的对称数字是一层层递归的 cascade 函数的 else 函数的 print 打印的，而1是 if 语句中的 print 打印的。

这个函数也可以这样写：

``` python
def cascade(n):
    print(n)
    if n >= 10:
        cascade(n//10)
        print(n)
```

如果要写一个相反的函数 inverse_cascade, 即打印形如

```bash
1
12
123
12
1
```

用 grow 函数打印前部分，用 shrink 函数打印后部分。

``` python
def inverse_cascade(n):
    grow(n)
    print(n)
    shrink(n)
    
def f_then_g(f, g, n):
    if n:
        f(n)
        g(n)
        
grow = lambda n: f_then_g(grow, print, n//10)
shrink = lambda n: f_then_g(print, shrink, n//10)
```

#### Question: Tower of Hanoi

> The **Tower of Hanoi** (also called the **Tower of Brahma** or **Lucas' Tower** and sometimes pluralized as **Towers**) is a [mathematical game](https://en.wikipedia.org/wiki/Mathematical_game) or [puzzle](https://en.wikipedia.org/wiki/Puzzle). It consists of three rods and a number of disks of different sizes, which can slide onto any rod. The puzzle starts with the disks in a neat stack in ascending order of size on one rod, the smallest at the top, thus making a [conical](https://en.wikipedia.org/wiki/Cone) shape.
>
> The objective of the puzzle is to move the entire stack to another rod, obeying the following simple rules:
>
> 1. Only one disk can be moved at a time.
> 2. Each move consists of taking the upper disk from one of the stacks and placing it on top of another stack or on an empty rod.
> 3. No larger disk may be placed on top of a smaller disk.
>
> Source: https://en.wikipedia.org/wiki/Tower_of_Hanoi

我们可以直接用递归“让它自己解决”。

```python
def move_disk(disk_number, from_peg, to_peg):
    print("Move disk" + str(disk_number) + " from peg " \
    + str(from_peg) + " to peg " + str(to_peg) + ".")
    
def solve_hanoi(n, start_peg, end_peg):
    if n == 1:
        move_disk(n, start_peg, end_peg)  # 直接移过去
    else:
        spare_peg = 6 - start_peg - end_peg
        # 将除最底下的其他块用solve_hanoi从开始处移动到空处
        solve_hanoi(n - 1, start_peg, spare_peg)
        # 将最底下的块移动到目标处
        move_disk(n, start_peg, end_peg)
        # 将除最底下的其他块用solve_hanoi从空处移动到目标处
        solve_hanoi(n - 1, spare_peg, end_peg)
```

#### Question: Counting Partitions

> Q: The number of partitions of a positive integer n, using parts up to size m, is the number of ways in which n can be expressed as the sum of positive interger parts up to m in increasing order.

> Example: count_partitions(6, 4)
>
> 2 + 4 = 6
>
> 1 + 1 + 4 = 6
>
> 3 + 3 = 6
>
> ....
>
> 1 + 1 + 1 + 1 + 1 + 1 = 6

![image-20210122212214585](../images/Recursion/image-20210122212214585.png)

```python
def count_partition(n, m):
    if n == 0:
        return 1
    elif n < 0:
        return 0
    elif m == 0:
        return 0
    else:
        with_m = count_partition(n-m, m) # or (n-m, min(m, n-m)) 2 4 --> 2 2
        without_m = count_partition(n, m-1)
        return with_m + without_m
```



### 互递归

Luhn算法同样是需要将各数字位相加，只是对偶数位要做些处理而已。

首先构造其递归相加函数：

``` python
def luhn_sum(n):
    if n < 10:
        return n
    else:
        all_but_last, last = split(n)
        # 与sum_digits的不同在于对偶数位做的处理不同
        # 形成了两个函数互相递归的形式
        # 这里的last是奇数位
        return luhn_sum_double(all_but_last) + last
```

``` python
def luhn_sum_double(n):
    all_but_last, last = split(n)
    luhn_digit =  sum_digits(2 * last)  # 这里的last为偶数位被加倍了
    if n < 10:
        return luhn_digit
    else:
        # 让luhn_sum将处理all_but_last里的最后一位也就是奇数位
        return luhn_sum(all_but_last) + luhn_digit
```

``` bash
>>> luhn_sum(32)
>>> 8  // 3 * 2 + 2 = 8
```

为了完成不同的处理逻辑，这个例子里 luhn_sum 函数与 luhn_sum_double 函数相互递归，这就是所谓的**互递归(mutual recursion)**。

### 树递归

如果一个函数进行了不止一次的递归调用，就叫做树递归(tree recursion)，典型的是递归版的斐波拉契函数：

``` python
def fib(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fib(n - 1) + fib(n - 2)
```

![fib](../images/Recursion/fib.svg)

一般来说，当你在同一时间进行多种运算时，会需要用到树递归。此外，普通递归可以很容易地与迭代形式转化，但是树递归很难写成迭代形式。

#### Question:   count_stair_ways  

> Q1: You want to go up a flight of stairs that has n steps. You can either take 1 or 2 steps each time. How many different ways can you go up this flight of stairs? Write a function count_stair_ways that solves this problem. Assume n is positive.  

这里可以用 count_stair_ways(n) = count_stair_ways(n-1) + count_stair_ways(n-2) 来递归计算走楼梯方式的次数，其中`count_stair_ways(n-1)`代表一步走上第n阶的方式次数，`count_stair_ways(n-2)`代表两步走上第n阶的次数。

``` python
def count_stair_ways(n):
    if n == 1:
        return 1
    elif n == 2:
        return 2
    else:
        return count_stair_ways(n-1) + count_stair_ways(n-2)
```

> Q2:   Consider a special version of the count_stairways problem,
> where instead of taking 1 or 2 steps, we are able to take up to and including k steps at a time. Write a function count_k that figures out the number of paths for this scenario. Assume n and k are positive.  

例如 n = 10, k = 3，count_k(10, 3) 可以倒推为：

1. 可能之前走了9个阶梯，最后一步为1
2. 可能之前走了8个阶梯，最后一步为2
3. 可能之前走了7个阶梯，最后一步为3

即 count_k(10, 3) = count_k(9, 3) + count_k(8, 3) + count_k(7, 3)，依次类推，count_k(9, 3) = count_k(8, 3) + count_k(7, 3) + count(6, 3)。最终收敛为 count(1, 3) = 1。

在实现上，当 n < k 时，会生成 n-k, n-k+1, ..., 0, ..., n-1 等作为第一个参数，这时候，令 n == 0 时 return 1，令 n < 0 时 return 0 即可得出正确的结果。

主函数：

``` python
def count_k(n, k):
    if n == 1:  # 一步
        return 1
    # 若n < k之类，
    # count(2,3)=count(1,3)+count(0,3)+count(-1,3)=2
    # 
    # count(3,4)=count(2,4)+count(1,4)+count(0,4)
    # count(2,4)=count(1,4)+count(0,4)=2
    if n == 0:
        return 1
    elif n < 0:
        return 0
    else n > 1:
        return loop_cur(count_k, n, k)
```

借助了一个 loop_cur 函数来生成递归相加的表达式：

``` python
def loop_cur(f, n, k):
    i = k
    result = 0
    while i:
        now += f(n-i, k)
        i = i - 1
    return result
```

