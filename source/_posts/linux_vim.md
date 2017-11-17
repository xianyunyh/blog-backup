title: Linux vim操作
categories: Linux
tags: [vim]
description: Linux vim
---



## linux关机和终端切换命令
Vim是一个类似于Vi的文本编辑器，不过在Vi的基础上增加了很多新的特性
<!--more-->

> vim 文件名  打开文件
  
	vim a.txt 	

### vim编辑器的三种模式。
> 命令模式 通过一些命令进行快捷操作的
> 编辑模式 可以输入
> 尾行模式  保存退出等操作

### 模式的切换 
vi打开后进入命令模式。
可以用四种方式进入编辑模式。

	i insert 光标停留在当前位置
	s select 清空光标对应的字符 并进去编辑模式
	o  进入编辑模式，并换行
	a  右移一个字符，然后再进入编辑模式

### 尾行模式

	ecs->冒号:
	:w保存
	:q 退出
	:q! 强制退出
	:wq保存退出

### vi命令模式下命令模式

#### 字符移动 ####

	h左移动
	l右移动

#### 单词级别移动 ####

	w移动到下个单词首
	e 移动本单词词尾
	b 移动本单词首

#### 行级别移动

	$ 移动到行尾
	0移动到行首
	j下移一行
	k上移一行

#### 段级别移动 

	{上移一段
	}下移一段

#### 屏级别移动

	H移动本屏幕第一行
	L屏幕的结尾
	G 跳到文档的结尾
	1G 跳到文档的开始
	光标跳到哪就能删到哪
	用d+光标快捷移动键 删除。
### 文本操作

	使用u撤销
	用x删除一个字符
	
	删除整行， dd
	
	复制和粘贴
	复制用yy
	复制三行 y3y
	粘贴 使用yp
	
	set nu 显示行号
	复制整段 y{
	用y使用光标快捷键进行快速复制