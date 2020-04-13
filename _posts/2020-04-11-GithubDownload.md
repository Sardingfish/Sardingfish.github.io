---
layout: post
title:  "GithubDownload"
date:   2020-04-11
categories: Tips
tags: Github
author: Jason Ding
---

## Github Repositories 下载速度过慢/失败问题

最近因为下载[Net_Diff](https://github.com/YizeZhang/Net_Diff)一直失败关注到这个问题，在网上查询发现是因为github的CDN(Content Distribute Network, 内容分发网络)被屏蔽了，导致DNS解析返回错误的结果，使得在本地环回，速度变慢。为了解决该问题，以下总结并测试了网络上查到的几类方法。



#### **方法1：仓库导入“[码云Gitee](https://gitee.com/)”，后从码云下载**

此方法推荐的人比较多，下速度也比较快，但是，对较大的仓库不太友好......

测试结果：失败，原因：仓库太大，需要升级到企业版才能导入。



#### **方法2：修改hosts文件**

- 打开网站[https://www.ipaddress.com/](https://www.ipaddress.com/);

- 分别在上面打开的网页中查找以下域名的IP地址；

  ```
  github.com
  github.global.ssl.fastly.net
  assets-cdn.github.com
  documentcloud.github.com
  gist.github.com
  help.github.com
  nodeload.github.com
  raw.github.com
  status.github.com
  training.github.com
  githubuser.github.com
  avatars1.githubusercontent.com
  codeload.github.com
  ```

  

- 将上面的IP地址添加到本地hosts文件中(C:\Windows\System32\drivers\etc\hosts for Win; \etc\hosts for Linux);

- 打开cmd，在cmd命令行输入：ipconfig/flushdns (刷新dns).

第二步的网址数量较多，而且每次查询地址都可能会变化，有人用python写了个脚本，可以自动查询这些ip并填入hosts文件及刷新本地DNS, 这里给出链接：[https//github.com/jvxiao/speed-github](https//github.com/jvxiao/speed-github).

测试结果：在Win和Linux平台均有效，下载速度从2~5kb/s，提升到了20~30kb/s，然而效果有限，对于比较大的仓库来说，还是很慢......



#### **方法3：使用一些在线代下载工具**

这里给出测试有效果的两个工具：

- [https://g.widora.cn/](https://g.widora.cn/)
- [https://gh.api.99988866.xyz/](https://gh.api.99988866.xyz/)

测试结果：失败，下载速度能达到60~80kb/s，但是两种工具下载下来的压缩包不完整且损坏无法解压。



最后，打开了科学上网工具某灯，从下载到完成<10s，果然，科学上网才是第一生产力......