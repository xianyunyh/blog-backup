title: Windows下AWstats安装教程
categories: linux
tags: [awstats]
description: AWStats是一个免费、功能强大、特性丰富的日志分析工具
---

# Windows下AWstats安装教程 #
 	
>AWStats是一个免费、功能强大、特性丰富的日志分析工具，它能分析由WEB、STREAMING、FTP、MAIL等服务生成的日志，并生成先进的统计图表。AWStats作为CGI或从命令行运行，在数个图形网页中显示你日志中包含的所有可能信息。它利用一部分档案资料就能经常很快地处理大量日志档案。它能分析的日志文件来自从各大服务器工具，如 Apache、nginx日志文件 (NCSA combined/XLF/ELF log format or common/CLF log format)、WebStar、IIS (W3C日志格式)及许多其他Web、Proxy(代理服务器)、Wap、流服务器、邮件服务器和一些FTP服务器。

<!--more-->
以下是余下全文

## 下载软件包
>下载activeperl AWStats需要perl的环境。下载地址[http://www.activestate.com/activeperl/downloads](http://www.activestate.com/activeperl/downloads)

>下载awstats 目前最新版本是7.4 [https://sourceforge.net/projects/awstats/?source=navbar](https://sourceforge.net/projects/awstats/?source=navbar "https://sourceforge.net/projects/awstats/?source=navbar")

## 安装activeperl

> 安装很简单，直接下一步即可。

## 安装awstats 

	解压到d盘 打开awstats-7.4 发现有三个文件夹
	
	进入到tools目录下找到 awstats_configure.pl 运行。
	
	第一次需要你输入你本地的apache的路径。
	
	第二次需要你输入你apache的配置的路径
	
	然后就一直按enter即可

## 修改站点配置 

>修改perl的路径，找到wwwroot/cgi-bin下的awstats.pl 找到第一行

	将#!/usr/bin/perl 修改为 你的perl的路径 我的是c:/perl