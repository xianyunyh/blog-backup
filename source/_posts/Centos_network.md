title: Centos配置网络
categories: Linux
tags: [Linux]
description: Centos配置网络
---

# Centos配置网络
##配置文件


-  **/etc/hosts** 完成ip和主机名的映射功能 类似windows的hosts文件

- **/etc/resolv.conf** 配置DNS

<!--more-->

- /etc/sysconfig/network-script/ifcfg-eth*X* 设置对应网口的信息 
###1. /etc/hosts的配置如下 ###

	 127.0.0.1   localhost
### 2./etc/resolv.conf的配置 ###

> 配置参数一般为nameserver domain search sorlist


1. nameserver 指定dns服务器的ip地址
2. domain 定义本地域名的信息，一般为localhost
3. serach 定义域名的搜索列表
4. sortlist 对gethostbyname返回的地址进行排序


> 常用的配置一般是nameserver


		nameserver 8.8.8.8
		nameserver 8.8.4.4


### 3 ifcfg-eth'X'配置对应的网口###

> 文件所在的位置 /etc/sysconfig/network-script/ifcfg-eth0


设置对应网口的IP等信息（比如第一个就是ifcfg-eth0）

	DEVICE="eth0" #设备名 不要随便改
	BOOTPROTO="static"#开机协议 常见的三个参数static(静态) none dhcp(自动获取)
	BROADCAST="192.168.0.255"#广播地址
	HWADDR="00:16:36:1B:BB:74"#mac地址，不要改动
	IPADDR="192.168.0.100"#本机ip地址
	NETMASK="255.255.255.0"#子网掩码
	ONBOOT="yes"#是否开机自启动
	GATEWAY="192.168.0.1"#网关

### 常用的命令 ###


> ifconfig 查看网络的信息

> ifconfig eth0 up/down 开启或者关闭eth0网卡

> service network start/stop/restart 启动/关闭/重启网络

