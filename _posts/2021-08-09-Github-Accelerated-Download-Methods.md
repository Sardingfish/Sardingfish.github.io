---
layout: post
title:  "Github accelerated download methods"
date:   2021-08-09
categories: Tips
tags: Github
author: Jason Ding
---

* content
{:toc}
Github是程序猿们最常访问的网站，然而近年来其在国内的访问以及下载速度变得特别慢，有时候着急下载软件，而手头又没有合适的工具，或者月底了工具限流，就非常头疼。为了方便下载访问，这里以下载[Net_Diff](https://github.com/YizeZhang/Net_Diff)为例，试验了网上的几种加速方法，总结整理了可靠的几个，亲测有效~。



#### **镜像访问**

Net_Diff的github地址为：

```
https://github.com/YizeZhang/Net_Diff
```

直接打开经常显示“无法打开此网站”，有时候即使打开了，下载速度也特别慢，这个时候，可以将地址中的`github.com`换成下面的两个：

```
https://hub.fastgit.org/YizeZhang/Net_Diff

https://github.com.cnpmjs.org/YizeZhang/Net_Diff
```

就能打开了，而且下载速度也能达到3-4M/s，能够比较快地完成下载。



#### **通过码云Gitee中转下载**

首先需要注册一个码云（Gitee）账号，然后在码云中选择”从Github/GitLab导入仓库“，导入Net_Diff的Github仓库地址，等待导入完成，即可从码云下载Net_Diff，下载速度也非常快。



#### **谷歌浏览器加速插件”Github加速器“**

通过安装谷歌浏览器插件”Github加速器“，打开Net_Diff仓库后，可以在下载按钮”Code“旁边看到增加了”加速“按钮（如下图），点击加速通道给出的下载地址，也可以快速下载Net_Diff。

![image-20210809194309688](https://raw.githubusercontent.com/Sardingfish/Sardingfish.github.io/master/image/Others/githubacc.png)



#### **通过云服务器下载**

通过网页下载Net_Diff比较慢，但是在云服务器上下载速度却不慢，实测通过git clone在阿里云和腾讯云服务器上能达到1-2M/s的速度，而在课题组服务器（SHAO内网）甚至能达到20M/s的下载速度，科研利器啊！

```
git clone git clone https://github.com/YizeZhang/Net_Diff.git
```