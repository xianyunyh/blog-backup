title: Linux 下载安装FTP服务
categories: Linux
tags: [Ftp]
description: Linux 下载安装FTP服务
---


##Linux 下载安装FTP服务

###安装环境介绍

<!--more-->

> Linux发型版本:centos 6.4

> 为了减少不必要的麻烦。请先关闭selinux 

　关闭SELinux的方法：

　　修改/etc/selinux/config文件中的SELINUX="" 为 disabled ，然后重启。

　　如果不想重启系统，使用命令`setenforce 0`



1. 安装	


	    yum -y  install vsftpd


2. 添加ftp用户


		useradd -d /var/test -s /sbin/nologin test
	-d  /var/test 设置该用户的主目录
	-s /sbin/nologin 让该用户不能使用ssh登录


3. 设置ftp用户密码


	passwd test


2. 配置vsftpd

> vsftpd的配置文件在 /etc/vsftpd/
> 
###vsftpd配置文件


	Anonymous_enable=no (不允许匿名登陆)
	write_enable=YES//赋予可写入权限
	chroot_local_user=YES//锁定用户目录，ftp用户登录ftp只能在自己的目录下操作
	anon_upload_enable=NO
	anon_mkdir_write_enable=NO//禁止匿名用户的上传、新建目录权限
	dirmessage_enable=YES//允许ftp用户列出文件目录
	local_enable=YES
	write_enable=YES
	local_umask=022
	#chown_uploads=YES
	#chown_username=whoever
	#xferlog_file=/var/log/xferlog
	xferlog_std_format=YES
	#idle_session_timeout=600
	#data_connection_timeout=120
	#nopriv_user=ftpsecure
	#async_abor_enable=YES
	#ascii_upload_enable=YES#ascii上传
	#ascii_download_enable=YES
	#ftpd_banner=Welcome to blah FTP service.#欢迎语
	#chroot_list_file=/etc/vsftpd/chroot_list
	#ls_recurse_enable=YES
	listen=YES
	listen_port=21#端口
	#listen_ipv6=YES
	pam_service_name=vsftpd
	userlist_deny=yes#禁止userlist中的用户登录ftp
	userlist_file=/etc/vsftpd/user_list


### 修改/etc/pam.d/vsftpd ###
		

	auth       required     pam_listfile.so item=user sense=deny file=/etc/vsftpd/ftpusers onerr=succeed


> 把里面的deny改成allow


### 添加ftp登录用户 ###
> 向ftpusers中添加用户 每一行是一个用户


	vi /etc/vsftpd/ftpusers


### 更改防火墙


> 修改防火墙，放行21号端口	

	vi /etc/sysconfig/iptables
	增加以下
	-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT


### 重启防火墙和vsftpd ###


	service vsftpd restart
	service iptables restart
