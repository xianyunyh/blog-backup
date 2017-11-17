title: 添加nginx php-fpm 到系统启动项
categories: Linux
tags: [nginx]
description: 添加nginx php-fpm 到系统启动项
---


1. 在/etc/init.d/ 下建立nginx文件
2. 添加内容
<!--more-->

		#!/bin/sh 

		# nginx - this script starts and stops the nginx daemon 
		 
		# Source function library. 

		. /etc/rc.d/init.d/functions 
		 
		# Source networking configuration. 

		. /etc/sysconfig/network 
		 
		# Check that networking is up. 

		[ "$NETWORKING" = "no" ] && exit 0 
		 
		# 这里要根据实际情况修改

		nginx="/usr/local/nginx/nginx" 

		prog=$(basename $nginx) 
		 
		# 这里要根据实际情况修改

		NGINX_CONF_FILE="/usr/local/nginx/nginx.conf" 
		 
		[ -f /etc/sysconfig/nginx ] && . /etc/sysconfig/nginx 
		 
		lockfile=/var/lock/subsys/nginx 
		 
		start() { 
		    [ -x $nginx ] || exit 5 

		    [ -f $NGINX_CONF_FILE ] || exit 6 

		    echo -n $"Starting $prog: " 

		    daemon $nginx -c $NGINX_CONF_FILE 

		    retval=$? 

		    echo 
		    [ $retval -eq 0 ] && touch $lockfile 
		    return $retval 
		} 
		 
		stop() { 
		    echo -n $"Stopping $prog: " 
		    killproc $prog -QUIT 
		    retval=$? 
		    echo 
		    [ $retval -eq 0 ] && rm -f $lockfile 
		    return $retval 
		    killall -9 nginx 
		} 
		 
		restart() { 
		    configtest || return $? 
		    stop 
		    sleep 1 
		    start 
		} 
		 
		reload() { 
		    configtest || return $? 
		    echo -n $"Reloading $prog: " 
		    killproc $nginx -HUP 
		    RETVAL=$? 
		    echo 
		} 
		 
		force_reload() { 
		    restart 
		} 
		 
		configtest() { 
		    $nginx -t -c $NGINX_CONF_FILE 
		} 
		 
		rh_status() { 
		    status $prog 
		} 
		 
		rh_status_q() { 
		    rh_status >/dev/null 2>&1 
		} 
		 
		case "$1" in 
		    start) 
		        rh_status_q && exit 0 
		        $1 
		        ;; 
		    stop) 
		        rh_status_q || exit 0 
		        $1 
		        ;; 
		    restart|configtest) 
		        $1 
		        ;; 
		    reload) 
		        rh_status_q || exit 7 
		        $1 
		        ;; 
		    force-reload) 
		        force_reload 
		        ;; 
		    status) 
		        rh_status 
		        ;; 
		    condrestart|try-restart) 
		        rh_status_q || exit 0 
		        ;; 
		    *)    
		      echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}" 
		        exit 2 
				esac
3. 修改其权限并开机启动
> 修改权限 chmod 755 /etc/init.d/nginx

4. 设置为开机启动

>chkconfig --add nginx

>chkconfig nginx on 

5. 备注

> service nginx start 

>service nginx stop 

> service nginx reload | restart


### 添加php-fpm的方法类似 脚本代码如下


		#! /bin/sh
		# Comments to support chkconfig on CentOS
		# chkconfig: 2345 65 37
		#
		set -e
		 
		PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
		DESC="php-fpm daemon"
		NAME=php-fpm
		DAEMON=/usr/local/php/sbin/$NAME
		 
		CONFIGFILE=/usr/local/php/etc/php-fpm.conf
		PIDFILE=/usr/local/php/var/run/$NAME.pid
		SCRIPTNAME=/etc/init.d/$NAME
		 
		# Gracefully exit if the package has been removed.
		test -x $DAEMON || exit 0
		 
		d_start() {
		  $DAEMON -y $CONFIGFILE || echo -n " already running"
		}
		 
		d_stop() {
		  kill -QUIT `cat $PIDFILE` || echo -n " not running"
		}
		 
		d_reload() {
		  kill -HUP `cat $PIDFILE` || echo -n " can't reload"
		}
		 
		case "$1" in
		  start)
		        echo -n "Starting $DESC is success"
		        d_start
		        echo "."
		        ;;
		  stop)
		        echo -n "Stopping $DESC is success"
		        d_stop
		        echo "."
		        ;;
		  reload)
		        echo -n "Reloading $DESC configuration..."
		        d_reload
		        echo "reloaded."
		  ;;
		  restart)
		        echo -n "Restarting $DESC is success"
		        d_stop
		        sleep 1
		        d_start
		        echo "."
		        ;;
		  *)
		         echo "Usage: $SCRIPTNAME {start|stop|restart|force-reload}" >&2
		         exit 3
		        ;;
		esac


 - **本文有任何错误，或有任何疑问，欢迎留言说明**。