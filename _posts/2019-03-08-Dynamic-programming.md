---
layout: post
title:  "Dynamic programming"
date:   2019-03-08
categories: Program
tags: Algorithm
author: Jason Ding
---

![](https://raw.githubusercontent.com/Sardingfish/Sardingfish.github.io/master/image/2019-03-08-Dynamic-programming/Po-Gcej.png)

这个问题是在知乎上发现的，之所以关注是因为此前有听导师讲过在PPP中卫星失锁后如何快速重新初始化是目前的一个难题，理解动态规划后发现这个难题可以归类为动态规划问题，故在此做一个记录。



**动态规划(Dynamic Programming)**是计算机科学领域的一个概念，它是一种特殊的分治思想，利用它可以实现时间复杂度的优化。Dynamic是“动态的”，Programming在这里理解为“表格法”，合起来就是用一张可变的表格来存储运算结果，以达到减少重复计算，快读获取结果的目的。下面以一个具体例子来说明：



> 有n个阶梯，一个人每一步只能跨一个台阶或者两个台阶，问这个人一共有多少种走法？



首先需要对这个问题进行抽象，n个阶梯，每个阶梯都代表一个节点，然后这n个节点之间会有一些桥梁把它们连接起来，这个问题就转化为从节点0到节点n一共有多少种走法？

![](https://raw.githubusercontent.com/Sardingfish/Sardingfish.github.io/master/image/2019-03-08-Dynamic-programming/problem.png)

假设我们已经知道了5到10的路径数，这个数就可以保存下来，当我们再次计算3到10或者4到10的时候，这个数就可以直接拿来用，我们需要做的只是计算3到5或者4到5的路径数，而5到10的路径数无需重复计算，大大节省了时间。

这样，我们可以创建一个数组a[]，专门用来存放x到10的路径数，初始值记为0，每当要计算x到10的路径数的时候，先查看一下a[x]是否大于零，若大于，则说明此前已经计算过了，可以直接取用。

更多的例子可以参考这里：[https://medium.com/@codingfreak/top-50-dynamic-programming-practice-problems-4208fed71aa3](https://medium.com/@codingfreak/top-50-dynamic-programming-practice-problems-4208fed71aa3)

