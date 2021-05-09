---
layout: post
title:  "Matlab installation on linux server"
date:   2021-05-09
categories: Linux
tags: Matlab
author: Jason Ding
---

* content
{:toc}
近期陆陆续续总共给4台服务器安装了Matlab，每次都要重新找教程，为了以后方便，这里记录以一下安装流程。




---

#### **解压和挂载**

Linux版本的Matlab压缩包内的文件通常包含两个iso镜像和一个破解包，破解包里面包含license文件。下面以R2018a版本为例，解压后的文件目录为：/home/username/packages/matlabR2018a，破解包解压后文件夹名为Crack，镜像挂载目录为/media

```shell
cd /home/username/packages/matlabR2018a
sudo mkdir /media/matlab
sudo mount -o loop R2018a_glnxa64_dvd1.iso /media/matlab/
```

---

#### **安装**

安装之前需要确定软件安装的目录，这里选定/usr/local为安装目录，注意这里下面命令中”-fileInstallationKey“后面的数字为Crack文件下readme.txt文件下的第一串数字。

```shell
sudo mkdir /usr/local/MATLAB
cd /media/matlab
sudo /media/matlab//install -mode silent -fileInstallationKey 09806-07443-53955-64350-21751-41297 -agreeToLicense yes -activationPropertiesFilehome /home/username/packages/matlabR2018a/Crack/license_standalone.lic -destinationFolderhome /usr/local/MATLAB
```

安装到一般的时候终端会显示提示消息，内容为：弹出dvd1并插入dvd2继续，这个时候新开一个终端，执行以下命令：

```shell
cd /home/username/packages/matlabR2018a
sudo mount -o loop R2018a_glnxa64_dvd2.iso /media/matlab/
```

执行后前一个终端会继续安装，直到完成安装。

---

#### **破解**

完成安装后执行以下命令：

```shell
sudo cp cd /home/username/packages/matlabR2018a/Crack/R2018a/bin/glnxa64/* /usr/local/MATLAB/R2018a/bin/glnxa64
sudo mkdir /usr/local/MATLAB/R2018a/licenses
sudo cp /home/username/packages/matlabR2018a/Crack/license_standalone.lic /usr/local/MATLAB/R2018a/licenses
cd /usr/local/MATLAB/R2018a/bin
./matlab -chome /usr/local/MATLAB/R2018a/licenses/license_standalone.lic
```

破解后键入matlab，查看是否安装成功。

---

#### **增加环境变量**

为了使得在任意目录下均能使用matlab命令，需要将matlab可执行程序加入环境变量：

```shell
sudo vim ~/.bashrc
export PATH=/usr/local/MATLAB/R2018a/bin:$PATH
source ~/.bashrc
```

---

#### **使用**

```shell
matlab -nodesktop -nosplash -nodisplay -r mscript # mscript为matlab脚本mscript.m，注意命令中不包含.m
```

此外，为了使得命令正常退出，matlab脚本中最后需要加上exit或者quit

对于带参数的matlab脚本，可以使用以下方式处理：

```
mcommand=mscript" "para1" "para2" "para3
matlab -nodesktop -nosplash -nodisplay -r "$mcommand"
```

注意用此方式传入matlab函数中的变量为字符型，如果参数是数字，可以在matlab脚本里通过str2num函数转换。

#### **参考**

http://blog.csdn.net/minione_2016/article/details/53313271

https://www.2cto.com/kf/201708/671460.html

https://blog.csdn.net/u014535579/article/details/78793028

[https://blog.csdn.net/u014577061/article/details/79408337](https://blog.csdn.net/u014577061/article/details/79408337)

[Linux下无图形界面安装Matlab - WOTGL - 博客园](https://www.cnblogs.com/vincent-vg/p/8053152.html)

[linux 服务器安装 Matlab - 程序员 翁笔站长联盟](https://www.wengbi.com/thread_93137_1.html)