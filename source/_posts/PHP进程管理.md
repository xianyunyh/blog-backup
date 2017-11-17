title: Php进程管理介绍
categories: PHP
tags: [PHP,cgi]
description: Php进程管理介绍
---


## 1、什么是CGI

CGI全称是“公共网关接口”(Common Gateway Interface)，HTTP服务器与你的或其它机器上的程序进行“交谈”的一种工具，其程序须运行在网络服务器上。

CGI可以用任何一种语言编写，只要这种语言具有标准输入、输出和环境变量。如php,perl,tcl等

<!--more-->

## 2、什么是FastCGI

FastCGI像是一个常驻(long-live)型的CGI，它可以一直执行着，只要激活后，不会每次都要花费时间去fork一次(这是CGI最为人诟病的fork-and-execute 模式)。它还支持分布式的运算, 即 FastCGI 程序可以在网站服务器以外的主机上执行并且接受来自其它网站服务器来的请求。

FastCGI像是一个常驻(long-live)型的CGI，它可以一直执行着，只要激活后，不会每次都要花费时间去fork一次(这是CGI最为人诟病的fork-and-execute 模式)。它还支持分布式的运算, 即 FastCGI 程序可以在网站服务器以外的主机上执行并且接受来自其它网站服务器来的请求。

　　FastCGI是语言无关的、可伸缩架构的CGI开放扩展，其主要行为是将CGI解释器进程保持在内存中并因此获得较高的性能。众所周知，CGI解释器的反复加载是CGI性能低下的主要原因，如果CGI解释器保持在内存中并接受FastCGI进程管理器调度，则可以提供良好的性能、伸缩性、Fail- Over特性等等。


## 3、FastCGI的工作原理

　　1、Web Server启动时载入FastCGI进程管理器（IIS ISAPI或Apache Module)

2、FastCGI进程管理器自身初始化，启动多个CGI解释器进程(可见多个php-cgi)并等待来自Web Server的连接。

　　3、当客户端请求到达Web Server时，FastCGI进程管理器选择并连接到一个CGI解释器。Web server将CGI环境变量和标准输入发送到FastCGI子进程php-cgi。

4、FastCGI子进程完成处理后将标准输出和错误信息从同一连接返回Web Server。当FastCGI子进程关闭连接时，请求便告处理完成。FastCGI子进程接着等待并处理来自FastCGI进程管理器(运行在Web Server中)的下一个连接。 在CGI模式中，php-cgi（PHP-CGI是PHP自带的FastCGI管理器。）在此便退出了。

在上述情况中，你可以想象CGI通常有多慢。每一个Web请求PHP都必须重新解析php.ini、重新载入全部扩展并重初始化全部数据结构。使用FastCGI，所有这些都只在进程启动时发生一次。一个额外的好处是，持续数据库连接(Persistent database connection)可以工作。
5、FastCGI的不足

因为是多进程，所以比CGI多线程消耗更多的服务器内存，PHP-CGI解释器每进程消耗7至25兆内存，将这个数字乘以50或100就是很大的内存数。


早期的 Web 服务，是基于传统的 CGI 协议实现的。每个发送到服务器的请求，都需要经过启动进程、处理请求、结束进程三个步骤，以至于访问量增大时，系统资源（如内存、CPU 等）开销也巨大，导致服务器性能下降甚至服务中断。

在 CGI 协议下，解析器的反复加载是性能低下的主要原因。如果让解析器进程长驻内存，那么它只需启动一次，就可以一直执行着，不必每次都重新 fork 进程，这就有了后来的 FastCGI 协议。

总结：

首先，初始化 FastCGI 进程管理器，并启动多个 CGI 解释器子进程；
接着，当请求到达 Web 服务器时，进程管理器选择并连接一个子进程，将环境变量和标准输入发送给它，处理完成后将标准输出和错误信息返还给 Web 服务器；
最终，子进程关闭连接，继续等待下一个请求的到来；

## 4、什么是PHP-FPM

PHP-FPM 是 PHP 针对 FastCGI 协议的具体实现，也是 PHP 在多种服务器端应用编程端口（SAPI：cgi、fast-cgi、cli、isapi、apache）里使用最普遍、性能最佳的一款进程管理器。

PHP-FPM 这种模型是非常典型的多进程同步模型，意味着一个请求对应一个进程线程，并且 IO 是同步阻塞的。所以尽管 PHP-FPM 维护着独立的 CGI 进程池、系统也可以很轻松的管理进程的生命周期，

受制于服务器的硬件设施，PHP-FPM 需要指定合理的 php-fpm.conf 配置：