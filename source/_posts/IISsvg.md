title: 让IIS支持SVG的方法
categories: Windows
tags: [IIS]
description: 让IIS支持SVG的方法
date: 2016-02-11 00:00:02 
---

## 让IIS支持SVG的方法

> 在iis上默认不支持 

<!--more-->

1. 先打开IIS-找到你网站点右键属性-HTTP头-点击MIME类型

![](http://www.198zone.com/uploadfile/2014/0708/20140708094728370.jpg)

2. 单击新建，按照如下内容输入

		扩展名：.svg
		MIME类型：image/svg+xml

![](http://www.198zone.com/uploadfile/2014/0708/20140708094908386.jpg)