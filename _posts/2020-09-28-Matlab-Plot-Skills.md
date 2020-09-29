---
layout: post
title:  "Matlab Plot Skills"
date:   2020-09-28
categories: Tool
tags: Matlab
author: Jason Ding
---

* content
{:toc}
Matlab应该是大部分人最早接触到的除了Excel之外的画图工具，相比于其他方法，Matlab画图快捷方便，网上的资源也非常多，能够非常快速地帮助我们完成结果的可视化。但是，要使用Matlab画出足够精美的图，为论文增添光彩，有时候并不那么容易，本文总结了一些Matlab画图的技巧。




---

##### Figure

**设置图幅figure大小**

```matlab
set(gcf,'Units','centimeter','Position',[10 5 15 7.8]); % 第1，2个参数为figure位置，第3，4个参数为figure大小
```

**设置子图subplot的大小（使紧凑）**

```matlab
set(gca,'position',[0.03 0.07 0.98 0.88]); % 前两个参数为子图在figure中的位置，第3，4个参数代表子图大小，数字代表百分比
```

**给特定区域加背景色（填充面横坐标，填充面纵坐标，填充颜色rgb值）**

```matlab
patch([0,3,3,0],[0,0,12,12],[0.1,0.1,0.1],'FaceAlpha',.2,'EdgeColor','none');
```

**图幅消除白边**

```matlab
set(gca, 'LooseInset', [0,0,0,0]);
```

```matlab
set(gca,'LooseInset',get(gca,'TightInset'))
```

**坐标轴断开breakyaxis和breakxaxis**

以时间序列为例：有时候存在时间上一段时间数据的缺失，造成图中大片空白，又或者一些时间对应的数值远大于其他数值，画出的图中无法显示其他数值的相对大小，就需要对x轴或者y轴折叠，或者说断开。

Example：

```matlab
a=[2,5,9,890];
bar(a);
axis([0.5,4.5,0,900]);
breakyaxis([10 880]);
```

**subplot部分子图缺失问题**

用subplot绘制组图时，我们经常用set(gca,'position',[0.78 0.12 0.18 0.64])来使得子图变得紧凑，这样做有时候会使得绘制出来的图出现部分子图缺失，这是由于subplot会把整个figure分割为设定好的的等大网格，一旦某副子图set(gca,'position',[0.78 0.12 0.18 0.64])所设定的位置和大小越界，就会出现缺失。

**subplot绘制非等大小子图**

正常情况下使用subplot画图时每幅子图大小一致，有时候我们需要绘制含有非等大子图的组图，比如“品”或者倒“品”字型的组图，这个时候我们可以使用subplot将figure分割地细一些，比较大的子图占据多个网格，如subplot(2, 2, [1, 2])，来实现非等大子图绘制。

以下图为例，左边为：

```matlab
subplot(2,3,[1,2,4,5]);...
subplot(2,3,3]);...
subplot(2,3,6]);...
```

右边为：

```matlab
subplot(2,3,1]);...
subplot(2,3,4]);...
subplot(2,3,[2,3]);...
subplot(2,3,[5,6]);...
```



![](https://raw.githubusercontent.com/Sardingfish/Sardingfish.github.io/master/image/Others/subplot.png)

---

##### **Tick**

**坐标轴标值旋转** （例如以年月日为横坐标的时候，横放会相互重叠，竖放非常占空间，斜放刚刚好）

```matlab
set(gca, 'XTickLabelRotation', 45);
```

**坐标刻度向外**

```matlab
set(gca, 'tickdir', 'out');
```

**坐标刻度长短(majortick, minortick)**

```matlab
set(gca, 'ticklength', [0.02,0.01]);
```

**显示副刻度**

```matlab
set(gca, 'xminortick', 'on', 'yminortick', 'on');
```

---

##### **Legend**

**legend位置**

```matlab
legend({'str1','str2','strn'},'Location','SouthEast');
```

**legend横排**

```matlab
hl = legend('str1','str2','strn');
set(hl,'Orientation','horizon');
```

**legend不显示外框** 

```matlab
set(hl,'Box','off');
```

---

##### Links

[https://ww2.mathworks.cn/help/matlab/creating_plots/types-of-matlab-plots.html](https://ww2.mathworks.cn/help/matlab/creating_plots/types-of-matlab-plots.html)

[https://ww2.mathworks.cn/products/matlab/plot-gallery.html](https://ww2.mathworks.cn/products/matlab/plot-gallery.html)

[https://mp.weixin.qq.com/s?__biz=MzU4OTcyOTg0MA==&mid=2247483686&idx=1&sn=64a9f0dd9197c6e896ef527fffeaf932&chksm=fdc85879cabfd16f4d1a3b773c86547e35afa79ce296a88dcfda32de2495399c07424d7caa61&scene=21#wechat_redirect](https://mp.weixin.qq.com/s?__biz=MzU4OTcyOTg0MA==&mid=2247483686&idx=1&sn=64a9f0dd9197c6e896ef527fffeaf932&chksm=fdc85879cabfd16f4d1a3b773c86547e35afa79ce296a88dcfda32de2495399c07424d7caa61&scene=21#wechat_redirect)

