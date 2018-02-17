title: Lnmp环境的配置
categories: Linux
tags: [lnmp]
description: linux下lnmp环境的配置
date: 2016-02-09 00:00:02 
---

## LNMP配置 #
> 安装环境 Centos 6.4
		
>安装gcc编译器以及相关工具

		yum -y install gcc gcc-c++  autoconf  automake libtool pcre pcre-devel
<!--more-->
## 安装Nginx ##
- nginx 源码包下载地址：[http://nginx.org/download/nginx-1.9.1.tar.gz](http://nginx.org/download/nginx-1.9.1.tar.gz)
- 安装nginx 依赖库

	yum -y install zlib zlib-devel openssl openssl-devel pcre pcre-devel


- 编译安装
		

		> wget -c http://nginx.org/download/nginx-1.9.1.tar.gz
		>tar zxvf nginx-1.9.1.tar.gz 
		>cd nginx-1.9.1
		>.configure --prefix=/usr/local/nginx 
		>make && make install



- 运行nginx


		>/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf


- 强制停止nginx 


 		>pkill -9 nginx


- nginx配置 平滑重启


		>/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf

		 
>修改防火墙 让80端口通过

		vi /etc/sysconfig/iptables
		#增加以下内容
		-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT

> 重启防火墙

		service iptables restart

## 编译安装php5.6  ##
> php5.6的源码包地址:[http://cn2.php.net/distributions/php-5.6.12.tar.gz](http://cn2.php.net/distributions/php-5.6.12.tar.gz)

	wget http://cn2.php.net/distributions/php-5.6.12.tar.gz	
	
> 安装前的准备 安装php依赖库

		yum -y install libmcrypt-devel mhash-devel libxslt-devel \
		libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 libxml2-devel \
		zlib zlib-devel glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel \
		ncurses ncurses-devel curl curl-devel e2fsprogs e2fsprogs-devel \
		krb5 krb5-devel libidn libidn-devel openssl openssl-devel


> 安装mcrypt 使用php mcrypt 前必须先安装Libmcrypt

		wget http://softlayer.dl.sourceforge.net/sourceforge/mcrypt/libmcrypt-2.5.8.tar.gz
		tar -zxvf libmcrypt-2.5.8.tar.gz
		cd /usr/local/src/libmcrypt-2.5.8
		./configure --prefix=/usr/local
		make
		make install


### 编译安装php

		>./configure --prefix=/usr/local/php \
		--enable-mbstring  --with-curl \
		--with-bz2  --with-zlib  \
		--enable-pcntl \
		--with-config-file-path=/usr/local/php/etc/ \
		--with-mhash --enable-zip  \
		--with-mysql=mysqlnd --with-mysqli=mysqlnd \
		--with-pdo-mysql=mysqlnd \
		--with-gd --with-jpeg-dir --with-freetype-dir --with-png-dir \
		--enable-fpm
		
		> make && make install

### 配置php
	
		cd /usr/local/php/etc/
		cp php-fpm.conf.defautl php-fpm.conf #改名
		groupadd www 
		useradd -g www www #添加www用户
		cd conf.d
		cp www.conf.d www.conf
		vi www.conf
		#把中间的user=nodbody group=nobody 改成user=www group=www

### php和Nginx整合

	vi /usr/local/nginx/conf/nginx.conf

1):在nginx.conf(Nginx主配置文件)中把前面的 *#* 去掉：

   	location ~ \.php$ {
            root           /www/;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }

2):增加默认页index.php
	
	 index index.html index.php;

## 编译安装mysql5.6

>  检测有没有旧版本

		rpm -qa |grep mysql
> 有的话使用下面的命令卸载

	
	rpm -e mysql   //普通删除模式
	rpm -e --nodeps mysql    // 强力删除模式，如果使用上面命令删除时，提示有依赖的其它文件，则用该命令可以对其进行强力删除

###安装mysql
> 安装编译所需要的包

	yum -y install make gcc-c++ cmake bison-devel  ncurses-devel
> 下载mysql源码包
下载地址http://mirrors.sohu.com/mysql/MySQL-5.6/mysql-5.6.25.tar.gz
	
	wget -c http://mirrors.sohu.com/mysql/MySQL-5.6/mysql-5.6.25.tar.gz
	tar zxvf mysql-5.6.25.tar.gz

	cd mysql-5.6.25
	cmake \
	-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
	-DMYSQL_DATADIR=/usr/local/mysql/data \
	-DSYSCONFDIR=/etc \
	-DWITH_MYISAM_STORAGE_ENGINE=1 \
	-DWITH_INNOBASE_STORAGE_ENGINE=1 \
	-DWITH_MEMORY_STORAGE_ENGINE=1 \
	-DWITH_READLINE=1 \
	-DMYSQL_UNIX_ADDR=/var/lib/mysql/mysql.sock \
	-DMYSQL_TCP_PORT=3306 \
	-DENABLED_LOCAL_INFILE=1 \
	-DWITH_PARTITION_STORAGE_ENGINE=1 \
	-DEXTRA_CHARSETS=all \
	-DDEFAULT_CHARSET=utf8 \
	-DDEFAULT_COLLATION=utf8_general_ci
	
	make && make install

> 添加mysql用户和组

	groupadd mysql
	useradd -g mysql mysql

> 修改/usr/local/mysql权限
	
	chown -R mysql:mysql /usr/local/mysql

### 初始化设置 ###

	cd /usr/local/mysql
	scripts/mysql_install_db --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data --user=mysql

> 在CentOS 6.4版操作系统的最小安装完成后，在/etc目录下会存在一个my.cnf，需要将此文件更名为其他的名字，如：/etc/my.cnf.bak，否则，该文件会干扰源码安装的MySQL的正确配置，造成无法启动。

### 启动mysql ###

> 添加服务，拷贝服务脚本到init.d目录，并设置开机启动

	cp support-files/mysql.server /etc/init.d/mysql
	chkconfig mysql on
	service mysql start  --启动MySQL

### 配置用户 ###
MySQL启动成功后，root默认没有密码，我们需要设置root密码。

设置之前，我们需要先设置PATH，要不不能直接调用mysql

修改/etc/profile文件，在文件末尾添加

	PATH=/usr/local/mysql/bin:$PATH
	export PATH

> 关闭文件，运行下面的命令，让配置立即生效

	source /etc/profile
> 执行下面的命令修改root密码

	mysql -uroot  
	mysql> SET PASSWORD = PASSWORD('123456');
> 配置防火墙

防火墙的3306端口默认没有开启，若要远程访问，需要开启这个端口

打开/etc/sysconfig/iptables

在“-A INPUT –m state --state NEW –m tcp –p –dport 22 –j ACCEPT”，下添加：

	-A INPUT -m state --state NEW -m tcp -p -dport 3306 -j ACCEPT
> 重启防火墙

	service iptables restart