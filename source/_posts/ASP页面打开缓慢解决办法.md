---
title: ASP页面打开缓慢解决办法
date: 2013-05-15 09:19:22
categories: APS.NET
tags:
comments:
---

  这里要说的是asp页面打开缓慢的问题。而不是打不开的问题。如果直接打不开的话，请先检查asp服务扩展是否开启
![img](https://img-blog.csdn.net/20130515224558438)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​
如果网站较多，请建立多个应用程序池。注意不要将.NET的应用程序池与asp的应用程序池混用。可能会造成不可预知的后果。
1、新建立一个应用程序池 
![图片](https://img-blog.csdn.net/20130515224729788)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​

名称自定义，建议1~2个asp程序公用一个应用程序池。
2、给网站新建虚拟目录

![图片](https://img-blog.csdn.net/20130515224748568)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)
虚拟目录的名称和本地名称相同。

![图片](https://img-blog.csdn.net/20130515224856559)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)
设置运行脚本权限。

![图片](https://img-blog.csdn.net/20130515225017588)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![图片](https://img-blog.csdn.net/20130515225040009)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)
如果仅仅是运行asp网站的话，。asp.net的版本设置为1.X即可


![图片](https://img-blog.csdn.net/20130515225108702)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​
虚拟目录的属性下设置指定应用程序池并且允许脚本资源访问。

![图片](https://img-blog.csdn.net/20130515225126436)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

应用程序池还可以进行各种优化设置
每次回收都将会提高服务器的相应。
![图片](https://img-blog.csdn.net/20130515225140373)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​

感谢网友[@爱情水晶](http://user.qzone.qq.com/7377114/) 的帮助。、 有什么不足还望多多指教。  