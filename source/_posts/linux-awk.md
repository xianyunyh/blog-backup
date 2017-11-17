title: linux_awk
date: 2017-04-22 22:58:51
tags:
---
> awk是一个强大的文本分析工具，相对于grep的查找，sed的编辑，awk在其对数据分析并生成报告时，显得尤为强大。简单来说awk就是把文件逐行的读入，以空格为默认分隔符将每行切片，切开的部分再进行各种分析处理

1. 使用语言

	awk '{pattern + action}' {filenames}
   
2.  调用方式

	1.命令行方式
    awk [-F  field-separator]  'commands'  input-file(s)
    其中，commands 是真正awk命令，[-F域分隔符]是可选的。 input-file(s) 是待处理的文件。
    在awk中，文件的每一行中，由域分隔符分开的每一项称为一个域。通常，在不指名-F域分隔符的情况下，	 默认的域分隔符是空格。

    2.shell脚本方式
    将所有的awk命令插入一个文件，并使awk程序可执行，然后awk命令解释器作为脚本的首行，一遍通过键	  入脚本名称来调用。
    相当于shell脚本首行的：#!/bin/sh
    可以换成：#!/bin/awk

    3.将所有的awk命令插入一个单独文件，然后调用：
    awk -f awk-script-file input-file(s)
    其中，-f选项加载awk-script-file中的awk脚本，input-file(s)跟上面的是一样的。
    
3. awk编程 
 
1. 统计日志中最多的ip


	 awk ‘{BEGIN a[$1]+=1;} END{for(i in a){print a[i],i}}|sort’