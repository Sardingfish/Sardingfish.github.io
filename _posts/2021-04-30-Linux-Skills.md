---
layout: post
title:  "Linux Skills"
date:   2021-04-30
categories: Tool
tags: Linux
author: Jason Ding
---

* content
{:toc}
本文收集整理了一些在特性场景下特别实用的Linux命令\配置\脚本。




---

#### **Configuration**

**服务器之间建立scp信任连接**

服务器之间传输数据通常使用scp来传输数据，但是scp每次使用都需要输入密码，当有大量scp需求或者含有scp命令的脚本需要被定时调度的时候，就可以在两台服务器之间建立信任连接来实现免密scp。

建立信任连接的步骤如下：

> 1. 登录服务器A，执行命令ssh-keygen -t rsa生成公私钥（选项全部回车），存储公私钥的文件夹为~/.ssh，文件名分别默认为id_rsa和id_rsa.pub
> 2. 将生成的公钥文件(id_rsa.pub)内容复制到服务器B的~/.ssh/authorized_keys文件中，若无该文件则创建一个

注意：执行上述过程后，相当于服务器B向服务器A敞开了大门，及服务器B信任服务器A，相当于服务器A拥有了不受限制的从服务器B获取数据和写入数据的权限。若需要服务器A信任服务器B，方法相同。

---



**服务器保存远程服务器登录信息**

通常我们不会只有一台服务器，在多个服务器之间切换的时候需要记住一堆服务器地址，尤其在缺乏终端模拟软件（如Xshell和SecureCRT等）的时候，比如团队合作时在他人计算机上操作或者WSL下在多台远程服务器见切换的时候。此时若将登录信息保存在服务器上，无论在哪一台上操作，都能够非常方便地进行切换。服务器保存远程服务器登录信息的方法如下:

1. 打开配置文件

   ```shell
   vim~/.ssh/config
   ```

2. 在文件中加入远程服务器信息

   ```shell
   Host ali                          # 服务器名字
   User root                         # 登录用户名
   ServerAliveInterval 60            # 防止挂起过久断线
   Hostname xx.xxx.xxx.xx            # 服务器地址
   ```

3. 保存并推出

config文件可写入多台服务器信息，使用时直接通过服务器名称登录即可，以上面的为例，键入`ssh ali`后输入密码即可。

---



#### **Commands**

**Linux定时任务命令crontab**

```shell
crontab [-u username]　　　　//省略用户表表示操作当前用户的crontab
    -e      (编辑工作表)
    -l      (列出工作表里的命令)
    -r      (删除工作表)
    
# For details see man 4 crontabs
# Example of job definition:
# .---------------- minute (0 - 59)
# | .------------- hour (0 - 23)
# | | .---------- day of month (1 - 31)
# | | | .------- month (1 - 12) OR jan,feb,mar,apr ...
# | | | | .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# | | | | |
# * * * * * user-name command to be executed
```

examples

```shell
* * * * * myCommand                       // 每1分钟执行一次myCommand
3,15 * * * * myCommand                    // 每小时的第3和第15分钟执行
3,15 8-11 * * * myCommand                 // 在上午8点到11点的第3和第15分钟执行
3,15 8-11 */2  *  * myCommand             // 每隔两天的上午8点到11点的第3和第15分钟执行
3,15 8-11 * * 1 myCommand                 // 每周一上午8点到11点的第3和第15分钟执行
30 21 * * * /etc/init.d/smb restart       // 每晚的21:30重启smb
45 4 1,10,22 * * /etc/init.d/smb restart  // 每月1、10、22日的4 : 45重启smb
10 1 * * 6,0 /etc/init.d/smb restart      // 每周六、周日的1 : 10重启smb
0,30 18-23 * * * /etc/init.d/smb restart  // 每天18 : 00至23 : 00之间每隔30分钟重启smb
0 23 * * 6 /etc/init.d/smb restart        // 每星期六的晚上11 : 00 pm重启smb
* */1 * * * /etc/init.d/smb restart       // 每一小时重启smb
* 23-7/1 * * * /etc/init.d/smb restart    // 晚上11点到早上7点之间，每隔一小时重启smb
```

查看执行log：vim /var/spool/mail/username

注意：命令中使用到系统时间时，经常会使用%，而百分号%在crontab中表示换行，因此应当使用转义字符\%代替%

---



**终端复用工具tmux**

基本操作：

~~~shell
tmux                                # 新建一个session, 名字为0
tmux new -s session1                # 新建一个名为session1的session
tmux detach                         # 断开当前session
tmux ls                             # 查看全部session
tmux a -t session1                  # 连接到session1, 若只有一个session, tmux a即可
tmux rename -t session1 session2    # 重命名session1为session2
tmux kill-session -t session1       # 删除session1
~~~

在tmux状态下的session操作：（prefix为前置键，默认为Ctrl+b，按下prefix之后输入命令）

~~~shell
prefix $                            # 重命名当前session
prefix d                            # 断开当前session，等效于tmux detach
prefix s                            # 切换session
prefix :new                         # 新建session
~~~

关于Pane的操作

~~~shell
prefix %                            # 水平分割pane
prefix "                            # 数值分割pane
prefix o                            # 切换pane
prefix 上下左右箭头                   # 切换pane
prefix ctrl 上下左右箭头              # 调整pane大小
exit                                # 删除当前pane

~~~

Ctrl+b 的默认prfix在键盘中距离较大，不便于操作，也可以用以下方式修改Ctrl +b 为Ctrl + x

1. 在tmux下打开.tmux.conf : vim ~/.tmux.conf

2. 写入并保存

   ~~~shell
   unbind C-b
   set -g prefix C-x
   ~~~

3. 按下Ctrl + b + : 在地下出现黄条输入框输入source .tmux.conf 回车，设置成功！

参考：http://dawnguo.cn/posts/76651904.html

---



**下载工具wget**

wget下载文件夹中全部数据

```shell
   wget -c -r -np -k -L -p http://docs.openstack.org/liberty/install-guide-rdo/
```

在下载时，有用到外部域名的图片或连接。如果需要同时下载就要用-H参数。

```shell
   wget -np -nH -r –span-hosts www.xianren.org/pub/path/
```

   -c 断点续传

   -r 递归下载，下载指定网页某一目录下（包括子目录）的所有文件

   -nd 递归下载时不创建一层一层的目录，把所有的文件下载到当前目录

   -np 递归下载时不搜索上层目录，如wget -c -r www.xianren.org/pub/path/

没有加参数-np，就会同时下载path的上一级目录pub下的其它文件

   -k 将绝对链接转为相对链接，下载整个站点后脱机浏览网页，最好加上这个参数

   -L 递归时不进入其它主机，如wget -c -r www.xianren.org/

如果网站内有一个这样的链接： www.xianren.org，不加参数-L，就会像大火烧山一样，会递归下载www.xianren.org网站

   -p 下载网页所需的所有文件，如图片等

   -A 指定要下载的文件样式列表，多个样式用逗号分隔

   -i 后面跟一个文件，文件内指明要下载的URL

---



