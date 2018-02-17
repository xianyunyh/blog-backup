title: Linux下mysql主从配置
categories: linux
tags: [linux]
description: Linux下mysql主从配置
date: 2016-02-15 00:00:02 
---

# Linux下mysql主从配置

> 需要两台机器，安装mysql，两台机器要在相通的局域网内

<!--more-->
以下是余下全文

>主机A:192.168.1.100

>从机B:192.168.1.101

## 第一步 配置主数据库 ##

1.  配置授权用户 赋予从机权限，有多台丛机，就执行多次
  
    mysql>GRANT REPLICATION SLAVE ON *.* TO 'test'@'192.168.1.101' IDENTIFIED BY '123456';

2. 打开主机的my.cnf 配置


		server-id        = 1    #主机标示，整数
		log_bin          = /var/log/mysql/mysql-bin.log   #确保此文件可写 mysqlbinlog日志文件
		read-only        = 0  #主机，读写都可以
		binlog-do-db     = test   #需要备份数据，多个写多行
		binlog-ignore-db = mysql #不需要备份的数据库，多个写多行
 

3. 配置从机的数据库 打开my.cnf 


		server-id               = 2
		log_bin                 = /var/log/mysql/mysql-bin.log
		master-host     =192.168.1.100
		master-user     =backup
		master-pass     =123456
		master-port     =3306
		master-connect-retry=60 #如果从服务器发现主服务器断掉，重新连接的时间差(秒)
		replicate-do-db =test #只复制某个库
		replicate-ignore-db=mysql #不复制某个库


4. 同步数据库
 
把主数据库的数据同步到从数据库

5. 重启主数据库，重启从数据库。

6. 验证

在主机A中 
	
	mysql>show master status\G;
	
在主机B中

	mysql>show slave status\G;
