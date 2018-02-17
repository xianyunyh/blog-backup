title: Config的使用
categories: Laravel
tags: [php,laravel]
description: Config的使用
date: 2017-04-04 00:00:02 
---


## Laravel Config的使用

> 配置文件存放在app\config目录下，

<!--more-->

	use Illuminate\Support\Facades\Config;


	Config::get('app.timezone') //读取配置 app.php下的timezone的信息 
	
	Config::set('app.timezone','PRC');//设置