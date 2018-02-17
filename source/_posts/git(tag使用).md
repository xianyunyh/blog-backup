title: git tag的使用
categories: linux
tags: [git]
description: git tag的使用
date: 2017-02-10 00:00:02 
---

标签可以针对某一时间点的版本做标记，常用于版本发布。
<!--more-->
## git 标签的操作 

### 列出标签 
	git tag #列出仓库中的所有标签
	git tag -l 'v0.1.'#搜索符合模式的标签
### 打标签 ###

	git tag tagname
	如 git tag v0.0.1
### 添加标签备注

	tag -a v0.0.1 -m "v0.0.1标签"
### 切换标签 ###

	git checkout tagname
	如 git checkout v0.0.1
### 查看标签的信息 ###

	git show tagname
	如 git show v0.0.1

### 删除标签 ###

	git tag -d v0.0.1
### 标签发布 ###

	git push origin v0.0.1 #将v0.0.1标签提交到git服务器
	git push origin –tags # 将本地所有标签一次性提交到git服务器