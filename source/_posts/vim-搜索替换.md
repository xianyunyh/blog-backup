title: vim-search
categories: linux
tags: [vim]
description: git tag的使用
---


>  vi/vim 中可以使用 :s 命令来替换字符串 :s/search_string/replace_string/

<!--more-->

:s/vivian/sky/ 替换当前行第一个 vivian 为 sky

:s/vivian/sky/g 替换当前行所有 vivian 为 sky

:n,$s/vivian/sky/ 替换第 n 行开始到最后一行中每一行的第一个 vivian 为 sky

:n,$s/vivian/sky/g 替换第 n 行开始到最后一行中每一行所有 vivian 为 sky

> n 为数字，若 n 为 .，表示从当前行开始到最后一行

可以使用 # 作为分隔符，此时中间出现的 / 不会作为分隔符

:s#vivian/#sky/# 替换当前行第一个 vivian/ 为 sky/

:%s+/oradata/apras/+/user01/apras1+ （使用+ 来 替换 / ）： /oradata/apras/替换成/user01/apras1/


利用 :s 命令可以实现字符串的替换。具体的用法包括：

:s/str1/str2/ 用字符串 str2 替换行中首次出现的字符串 str1

:s/str1/str2/g 用字符串 str2 替换行中所有出现的字符串 str1

:.,$ s/str1/str2/g 用字符串 str2 替换正文当前行到末尾所有出现的字符串 str1

:1,$ s/str1/str2/g 用字符串 str2 替换正文中所有出现的字符串 str1

:g/str1/s//str2/g 功能同上

> 从上述替换命令可以看到：g 放在命令末尾，表示对搜索字符串的每次出现进行替换；不加 g，表示只对搜索
> 字符串的首次出现进行替换；g 放在命令开头，表示对正文中所有包含搜索字符串的行进行替换操作。

