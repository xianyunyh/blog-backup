title: Linux 常用的命令
categories: Linux
tags: [Ftp]
description: Linux 常用的命令
---



## scp命令

> linux 中的scp命令 传送文件到另外一台linux服务器上

<!--more-->

		命令格式 scp 本地文件 用户名@主机地址:远程路径
		scp file.zip root@192.168.1.100:/home/

##ssh 连接远程Linux服务器

> ssh 连接远程linux服务

	ssh 用户名@主机地址 
	如 ssh root@192.168.1.100

## netstat命令

> netstat这个命令常用在网络监控方面。利用这个命令，可以查看当前系统监听的服务和已经建立的服务，以及相应的端口、协议等信息。


	-a ：all，表示列出所有的连接，服务监听，Socket资料
	-t ：tcp，列出tcp协议的服务
	-u ：udp，列出udp协议的服务
	-n ：port number， 用端口号来显示
	-l ：listening，列出当前监听服务
	-p ：program，列出服务程序的PID