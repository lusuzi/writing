---
title: "Python Environment Diagram Question"
date: 2021-01-17T15:07:28+08:00
draft: false
dropCap: false
tags: [python, 习题]
---

要求根据此 Python 程序执行产生的环境变量关系图倒推原程序是什么。

![Python-Environment-Diagram-Question](../images/Python-Environment-Diagram-Question/Python-Environment-Diagram-Question.png)

首先，看到 flip 函数返回 flip 函数，flop 函数返回 flop 函数，返回的都是自己，但是 Global frame 下 flip 名称指向的是原 flop 函数，而 flop 名称指向的是 flip 函数。所以，可以推测出调用部分的第一行填写的是 `flip, flop = flop, flip`。

然后，看 frame f1，是 flip 函数被调用了，且其参数 flop 被赋值为1，由于 flop 函数是 flip，flip 函数是 flop 了，故 `flip(____)(3)`里先调用了 flop 函数，且其参数传的是1，即 flop(1) 。继续看 frame f1，本地变量 flip 指向的是个 lambda 函数，而且被返回了，其参数为 flip，故 flip 函数的 flip 变量被赋值的内容可先写为 `flip = lambda flip: `。我们看到 frame f2 是个 lambda 函数被调用了，且其父函数是 f1，可见 f1 返回的 lambda 函数赋值为 flip 然后接着就被调用了，由于 f2 返回值为3，lambda 函数可写为`flip = lambda flip: 3`；由 f2 内部的 flip 值为2可知 flop(1) 返回的 lambda 函数紧接着被调用时的参数为2，即 `flip(____)(3)`里填的应该是 flop(1)(2) 。

frame f3 调用的是 flop 函数，其参数 flip 值为3，这就是 flop(1)(2) 返回的值了。然后，返回了 flop 名称，其指向的是 flip 函数，接着被调用了，参数为3，即 `flip(flop(1)(2))(3)` --> flip(3)(3) --> flop(3)。flop(3) 再次调用 flip 函数，这就是 f4 frame，参数 flop 被赋值为3，但是返回的却是 None ？上一次调用时参数 flop 被赋值为1时返回的不是个 lambda 函数吗？行为不一致，是由 if 语句控制的，这里 if 的条件我们可以填 `flop == 3`（答案不唯一），然后内部语句填 `return None`。



