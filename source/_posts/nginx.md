title: Nginx的配置
categories: Linux
tags: [nginx]
description: nginx的配置的一些介绍
---


## Nginx的安装和配置

### 在windows下的安装。
> 在windows下的安装很简单，直接下载编译好的二进制文件解压，然后运行nginx.exe就可以了。
		
windows nginx下载地址 [http://nginx.org/download/nginx-1.9.1.zip](http://nginx.org/download/nginx-1.9.1.zip)

### 在Linux下的安装##

nginx 源码包下载地址：[http://nginx.org/download/nginx-1.9.1.tar.gz](http://nginx.org/download/nginx-1.9.1.zip)
<!--more-->
- 安装环境 Centos 

-  安装gcc编译器以及相关工具


		yum -y install gcc gcc-c++  autoconf  automake


- 安装nginx 依赖库

	yum -y install zlib zlib-devel openssl openssl-devel pcre pcre-devel

- 编译安装


		>tar zxvf nginx-1.9.1.tar.gz 
		>cd nginx-1.9.1
		>./configure --prefix=/usr/local/nginx 
		>make && make install

- 运行nginx



		>/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf


- 强制停止nginx 


 		>pkill -9 nginx


- nginx配置 平滑重启



		>/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf


##Nginx 配置


> nginx配置文件默认的位置在conf/nginx.conf 在编译的时候通过--conf-path= 指定

### Nginx 压缩配置

	  gzip on; #表示开启gzip压缩
	  gzip_min_length  1k; #最小的长度
	  gzip_buffers     4 32k;#缓冲大小
	  gzip_http_version 1.1;#对HTTP/1.1协议的请求才会进行gzip压缩
	  gzip_comp_level 2;#压缩等级是2
	  gzip_types       text/plain application/x-javascript text/css application/xml;#压缩文件的类型
	  gzip_disable "MSIE [1-6].";#对IE6的gzip压缩禁止压缩
		

### Nginx 虚拟主机配置
> nginx配置文件中 一个**server** {}表示一个虚拟主机
> **listen** 表示监听的端口 
>
> **root** 表示站点的目录
> 
> **index**  表示默认页
> **autoindex **表示是否开启站点目录列表

		server {
        	listen       8000;
        	server_name  somename  alias  another.alias;

        location / {
            root   html;
            index  index.html index.htm;
			autoindex on
       }
    }
#### 1. nginx基于ip的虚拟主机 ####
	
	#第一个虚拟主机
	server {
	        listen       80;
	        server_name 127.0.0.1
	
	        location / {
	            root   /www/a;
	            index  index.html index.htm;
	        }
	    }
	#第二个虚拟主机
		server {
		        listen       80;
		        server_name  127.0.0.2 #在线上请换成真实的ip
		
		        location / {
		            root   /www/b;
		            index  index.html index.htm;
		        }
		    }

  
#### 2.  nginx 基于端口的虚拟主机配置 ####
	
	#第一个虚拟主机
	server {
	        listen       800;
	        server_name 127.0.0.1 #在线上请换成真实的ip
	
	        location / {
	            root   /www/a;
	            index  index.html index.htm;
	        }
	    }
	#第二个虚拟主机
		server {
		        listen       801;
		        server_name  127.0.0.1 #在线上请换成真实的ip
		
		        location / {
		            root   /www/b;
		            index  index.html index.htm;
		        }
		    }

#### 3.  nginx基于域名的虚拟主机配置 ####
> 最常用的基于域名的 通过改变server_name


	#第一个虚拟主机
		server {
	        listen       80;
	        server_name www.a.com #在线上请换成真实的域名
	
	        location / {
	            root   /www/a;
	            index  index.html index.htm;
	        }
	    }
		#第二个虚拟主机
		server {
		        listen       801;
		        server_name  www.b.com #在线上请换成真实的域名
		
		        location / {
		            root   /www/b;
		            index  index.html index.htm;
		        }
		    }


### Nginx关于浏览器本地缓存的配置 ###

> 语法 expires [tine|day]

- 设置图片30天的缓存


	  location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$ 
	  {
	      expires 30d;      
	   }

- 设置css 1小时缓存
 
	 
	 location ~ .*\.(js/css)?$ 
	 {
	      expires 1h;      
	  }

### Nginx 关于400、500等页面的设置 ###
    error_page  404              /404.html;
	error_page  500 502 503 504  /50x.html;
### 外部配置文件的引入 ###

	可以通过include 指令  
	例如 include vhost.conf

### Nginx rewrite规则介绍 ###
> nginx rewrite 规则中相关的指令右if rewrite set return break 等，rewrite是关键的指令

- 当页面请求的url是以a目录的下的任意html文件 请求交给根目录下的b.php
> rewrite ^/a/(.*)\.html /b.php?act=$1 break; 

- **if 指令** 检查一个条件是否符合条件，如果符合执行大括号内的语句

		if(!-f $request_filename)
		{
			return 403;
		}
> -d  !-d 判断目录是否存在

> -f !-f 判断文件是否存在

> -e !-e 判断文件或者目录是否存在

> ~ 表示区分大小写的匹配

> ~* 表示不区分大小写的匹配

- **return 指令** 

> 返回状态码给客户端 常用的有404 403 200 等

- 常用的变量


		HTTP核心模块中可以使用的变量
		$args 这个变量等于请求行中的参数
		$binary_remote_addr 二进制格式的客户端地址
		$content_length 等于 客户端请求头中的 content-length
		$content_type 等于客户端请求头中的content-type
		$http_cookie客户端请求header头中的cookie变量
		$document_root 对当前请求所属的root指定设置的文档目录
		$host 客户端请求的主机名
		$remote_addr 客户端的ip地址
		$remote_port 客户端的端口
		$request_filename 请求的文件名路径
		$request_body 请求的主体内容
		$request_method  请求的方式
		$server_name 服务器主机名
		$server_posrt 请求到达服务器的端口
		$server_protocol 采用的协议 http1.0、http1.1

### fastcgi的配置

	fastcgi_buffers 4K #设置fastcgi进程返回信息的缓冲区的数量和大小
	fastcgi_buffer_size 4K  #设置fastcgi服务器响应头部的缓冲区大大小。默认是	4k 8k
	fastcgi_pass  9000 #指定fastcig 服务器监听的端口
	fastcgi_cache_methods  #设置哪些http请求被缓存

##PHP和Nginx 的整合
> PHP 脚本请求全部转发到 FastCGI处理. 使用FastCGI默认配置.

	location ~ \.php$ {
	            root           /www/;
	            fastcgi_pass   127.0.0.1:9000;
	            fastcgi_index  index.php;
	            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
	            include        fastcgi_params;
	        }

### 让nginx 支持pathinfo的方式 ###

		location ~ \.php(.*)$  {
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_split_path_info  ^((?U).+\.php)(/?.+)$;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            fastcgi_param  PATH_INFO  $fastcgi_path_info;
            fastcgi_param  PATH_TRANSLATED  $document_root$fastcgi_path_info;
            include        fastcgi_params;
        }
