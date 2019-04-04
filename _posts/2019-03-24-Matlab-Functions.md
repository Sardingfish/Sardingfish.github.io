---
layout: post
title:  "Matlab Functions"
date:   2019-03-24
categories: Tool
tags: Matlab
author: Jason Ding
---

最近完成了一个文件比对的任务，在这个过程中发现了一些实用强大的matlab函数，在这里做一下总结。

- **intersect**

  函数用法：

  ~~~matlab
  [C, Ia, Ib] = intersect(A, B)
  ~~~

  返回值C为A、B的交集，Ia、Ib为对应交集元素所在原数组的位置。

- **sortrows**

  函数用法：

  ~~~matlab
  B = sortrows(A)
  B = sortrows(A,column)
  [B,index] = sortrows(A,...)
  ~~~

  对矩阵行或表行进行排序，此 MATLAB 函数 基于第一列中的元素按升序对矩阵行进行排序。当第一列包含重复的元素时，sortrows 会根据下一列进行排序，并对后续的相等值重复此行为。


- **tabulate**

  函数用法：

  ~~~
  tbl = tabulate(x)
  ~~~

  统计一个数组中各数字（元素）出现的频数、频率。

  第一列：x矩阵中所有出现的元素；

  第二列：每个元素的在矩阵中出现的次数；

  第三列：每个元素占总元素数的百分比。

