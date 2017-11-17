title: Laravel的生命周期
categories: Laravel
tags: [php,laravel]
description: Laravel的生命周期和目录介绍
---



## Laravel的生命周期

**入口文件** 
public/index.php 这个文件是 Laravel 应用程序所有请求的进入点

**HTTP核心 **
应用程序的请求的会被送往 HTTP 核心或终端核心
app/Http/Kernel.php

<!--more-->
HTTP 核心也定义了一份 HTTP 中间件 清单，所有的请求在被应用程序处理之前都必须经过它们。这些中间件处理 HTTP session 的读写、验证 CSRF 令牌、决定应用程序是否处于维护模式，以及其它更多任务作

**服务提供者**

最重要的核心启动加载行为之一，是加载你的应用程序的 服务提供者。应用程序的所有服务提供者，都在 config/app.php 此配置文件的 providers 数组中被设置。首先，所有提供者的 register 方法会被调用，一旦所有提供者都被注册之后，boot 方法就会被调用。

**请求分派**

一旦应用程序被启动且所有的服务提供者都被注册之后，Request 将被移转给路由器进行分派。路由器会将请求分派给路由或控制器，并运行所有特定路由的中间件。


## 程序结构介绍

	app 目录，如你所料，这里面包含应用程序的核心代码。我们之后将很快对这个目录的细节进行深入探讨。
	
	bootstrap 目录包含了几个框架启动跟自动加载设置的文件。以及在 cache 文件夹中包含着一些框架在启动性能优化时所生成的文件。
	
	config 目录，顾名思义，包含所有应用程序的配置文件。
	
	database 目录包含数据库迁移与数据填充文件。如果你愿意的话，你也可以在此文件夹存放 SQLite 数据库。
	
	public 目录包含了前端控制器和资源文件（图片、JavaScript、CSS，等等）。
	
	resources 目录包含了视图、原始的资源文件 (LESS、SASS、CoffeeScript) ，以及语言包。
	
	storage 目录包含编译后的 Blade 模板、基于文件的 session、文件缓存和其它框架生成的文件。
		此文件夹分格成 app、framework，及 logs 目录。app 目录可用于存储应用程序使用的任何文件。
	    framework 目录被用于保存框架生成的文件及缓存。
		最后，logs 目录包含了应用程序的日志文件。
	
	tests 目录包含自动化测试。这有一个现成的 PHPUnit 例子。
	
	vendor 目录包含你的 Composer 依赖模块。


### APP目录

	app 目录附带许多个额外的目录，例如：Console、Http 和 Providers。
	可以将 Console 和 Http 目录试想为提供 API 进入应用程序的「核心」。
	HTTP 协定和 CLI 都是跟应用程序进行交互的机制，但实际上并不包含应用程序逻辑。换句话说，它们是两种简单地发布命令给应用程序的方法。
	Console 目录包含你全部的 Artisan 命令，而 Http 目录包含你的控制器、中间件和请求。

	Jobs 目录用于放置应用程序 可队列的任务。任务可以被应用程序放到队列中，
	也可以在当前请求生命周期内同步运行。
	
	Events 目录，如你所料，是用来放置 事件类 的。事件可以被用于当指定动作发生时，通知你应用程序的其它部分，提供了很棒的灵活性及解耦。
	
	Listeners 目录了包含事件的处理类。处理进程接收一个事件，并针对该事件运行逻辑。例如，UserRegistered 事件可能由 SendWelcomeEmail 侦听器处理。
	
	Exceptions 目录包含应用程序的异常处理进程，同时也是个处置应用程序抛出异常的好位置。

> 在 app 目录中的许多类可以通过 Artisan 命令生成。若要查看可以使用的命令，只要在命令行运行 php artisan list make 命令即可。