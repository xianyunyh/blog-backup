title: git免输入用户名提交
categories: linux
tags: [git]
description: git免输入用户名提交
date: 2017-04-20 00:00:02 
---


在~/下， touch创建文件 .git-credentials：

		touch .git-credentials

<!--more-->
# 用vim编辑此文件，

		vim .git-credentials

#输入

		git config --global credential.helper store




