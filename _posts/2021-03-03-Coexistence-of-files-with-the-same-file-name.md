---
layout: post
title:  "Coexistence of files with the same file name"
date:   2021-03-03
categories: Tool
tags: Linux
author: Jason Ding
---

* content
{:toc}
记录一次奇怪的同路径下**同名文件共存**问题

今天通过FileZilla往网页服务器传输数据（几张图片）的后发现网页中图片并未更新，查看FileZilla发现新上传的图片并没有把原先旧的替换掉，两者以相同的名字同时存在，amazing啊！

一番研究后发现旧文件未被替换是因为旧文件名中存在不可见字符"\015"，在文件系统中确实是两个文件。

但是，无论是删除命令rm，还是使用mv重命名，在对这些同名文件进行操作的时候均会报错，也就是说，要让网页正常显示新图片，只能返回上一层目录删除整个文件夹然后重新上传图片:joy::joy::joy:



---

![](D:\Github Repositories\Sardingfish.github.io\image\Others\ls1.png)

![ls2](D:\Github Repositories\Sardingfish.github.io\image\Others\ls2.png)

---

一番折腾发现原来最常用的ls命令原来还有这么多不熟悉的功能，以下做了一下简单的整理：

1. 列出指定文件夹下的所有文件及目录

   ```shell
   ls -lR /home/user/test_dir/
   ```

2. 按照时间顺序列出文件

   ```shell
   ls -lt             # 时间排序，越新的越靠前
   ls -ltr            # 时间排序，越新越靠后
   ```

3. 按照文件大小列出文件

   ```shell
   ls -lSh            # 文件大小排序，约大越靠前，也可以使用r使得越小越靠前，h表示以可读选项显示, 不加h则默认为单位为字节
   ```

4. 统计文件/文件夹数量

   ```shell
   ls -l | grep "^-" | wc -l    # 统计文件数量，其中：^- 表示以-开头，即普通文件
   ls -l | grep "^d" | wc -l    # 统计目录数量
   ```

5. 列出所有文件的绝对路径

   ```shell
   ls | sed "s:^:`pwd`/:"
   ```

6. 打印文件名中的C语言类型转义字符

   ```shell
   ls -bl
   ```

更多使用方法可以使用ls --help查看