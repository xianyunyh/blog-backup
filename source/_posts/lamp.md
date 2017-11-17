title: Lamp环境的配置
categories: Linux
tags: [lamp]
description: linux下lamp环境的配置
---

## LAMP配置 #
> 安装环境 Centos 6.4
		
>安装gcc编译器以及相关工具

		yum -y install gcc gcc-c++  autoconf  automake libtool pcre pcre-devel
<!--more-->
## 安装apache2.4.12 ##
> 所需要的软件 apache源码包、apr源码包、apr-util源码包

-  apache2.4.12 的源码包[http://mirrors.cnnic.cn/apache//httpd/httpd-2.4.12.tar.gz](http://mirrors.cnnic.cn/apache//httpd/httpd-2.4.12.tar.gz)
-  APR 1.5.2 源码包下载地址 [http://mirrors.cnnic.cn/apache//apr/apr-1.5.2.tar.gz](http://mirrors.cnnic.cn/apache//apr/apr-1.5.2.tar.gz "http://mirrors.cnnic.cn/apache//apr/apr-1.5.2.tar.gz")
-  APR-util 1.5.4 源码包下载地址 [http://mirrors.cnnic.cn/apache//apr/apr-util-1.5.4.tar.gz](http://mirrors.cnnic.cn/apache//apr/apr-util-1.5.4.tar.gz)

### 1.编译安装apr
		> wget -c http://mirrors.cnnic.cn/apache//apr/apr-1.5.2.tar.gz
		>tar -zxf apr-1.4.5.tar.gz
		>./configure --prefix=/usr/local/apr
		>make &&make install

###2.编译安装apr-util
		>wget -c http://mirrors.cnnic.cn/apache//apr/apr-util-1.5.4.tar.gz
		> tar zxvf apr-util-1.5.4.tar.gz
		>./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr/bin/apr-1-config
		> make && make install


###3.编译apache
	
	> wget -c http://mirrors.cnnic.cn/apache//httpd/httpd-2.4.12.tar.gz
	> tar zxvf tar zxvf httpd-2.4.12.tar.gz
	>./configure --prefix=/usr/local/httpd2 \
		--enable-so \
		--with-apr=/usr/local/apr  \
		--with-apr-util=/usr/local/apr-util  \
		-enable-modules=all \
		--enable-rewrite \
		--enable-mods-shared=all \
	>make && make install
	
> 添加服务，拷贝服务脚本到init.d目录

		 cp /usr/local/apache/bin/apachectl /etc/init.d/httpd
		service httpd start
		 
>修改防火墙 让80端口通过

		vi /etc/sysconfig/iptables
		#增加以下内容
		-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
> 重启防火墙

		service iptables restart

##编译安装php5.6  ##
> php5.6的源码包地址:[http://php.net/get/php-5.6.9.tar.gz/from/a/mirror](http://php.net/get/php-5.6.9.tar.gz/from/a/mirror)

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

		./configure --prefix=/usr/local/php \
		--enable-mbstring  --with-curl \
		--with-bz2  --with-zlib  \
		--enable-pcntl \
		--with-mhash --enable-zip  \
		--with-mysql=mysqlnd --with-mysqli=mysqlnd \
		--with-pdo-mysql=mysqlnd \
		--with-gd --with-jpeg-dir --with-freetype-dir --with-png-dir \
		--with-apxs2=/usr/local/apache/bin/apxs \
		
		> make &&make install

###php和apache整合

	vi /usr/local/http2/conf/httpd.conf
1):在httpd.conf(Apache主配置文件)中增加：

   	AddType application/x-httpd-php .php
2):找到下面这段话:

	<IfModule dir_module>
	    DirectoryIndex index.html
	</IfModule>
增加默认页index.php
	
	 DirectoryIndex index.html index.php

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