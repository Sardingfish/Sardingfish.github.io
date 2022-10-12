---
layout: post
title:  " Progress bar in linux "
date:   2022-05-20
categories: Program
tags: Linux
author: Jason Ding
---

在Linux运维的过程中，我们经常会接触、安装各种工具软件，在这些工具软件的安装或者运行过程中，经常遇到进度条。这时候我们会想，如何在自己写的程序中也添加进度条效果呢？（让程序看起来专业、高大上一点:-））这里记录一下如何在Linux下分别使用Shell和C实现进度条效果。



##### **Shell**

首先，进度条本质上是在同一个地方不断打印、刷新不同长度的字符串，外加一个百分比数字。也就是说，它本质上是一个循环，每一次循环过后，把光标恢复到该行最开始的位置，下一次循环，就可以在相同位置再次打印。在Shell中，光标恢复到行首可以使用`\r`来实现。

假设有一个需要2秒时间安装的程序，以下给出例子：

```shell
#!/bin/bash
i=0
str='#'
while [ $i -le 100 ]
do
    printf "[%-100s][%d%%]\r" $str $(($i))
    str+='#'
	let i++
	sleep 0.02
done
printf "\n\n"
echo "***********************"
echo "Installation completed!"
echo "***********************"
```

复杂程序安装有可能不是匀速，这时候可以根据安装过程中生成的文件或者log文件等来判定百分比并赋值给以上的i变量。

##### **C**

C语言与Shell类似，有一点不同的是，C语言中printf会把输出先存入缓冲区，缓冲区有3种缓冲策略，分别为无缓冲、行缓冲和全缓冲。无缓冲指的是数据不缓冲，直接打印，行缓冲指的是只要遇到换行符就打印，全缓冲指的是直到把缓冲区填满才打印。

在不额外操作的情况下，程序表现为全缓冲，但是我们需要的是每次循环都刷新一次。然而这里不需要换行符，也就不能通过换行符实现行缓冲。为了实现想要的效果，可以使用fflush强制刷新。以下为完整的C代码。

```c
#include <stdio.h>    
#include <string.h>    
#include <unistd.h>

void ProgBar()    
{
	int i = 0;    
	char str[101];  
	memset(str, '\0', sizeof(str));    
	        
	while(i <= 100)    
	{  
	   printf("[%-100s][%d%%]\r", str, i);                                  
	   fflush(stdout); 
	   str[i] = '#';
	   usleep(20000);
	   i++;  
	 }  
	 printf("\n");  
}

int main()                        
{                             
	  ProgBar(); 
      printf("***********************\n");
      printf("Installation completed!\n");
	  printf("***********************\n");
	  return 0;                    
}
```

