layout: web_layout
title: Web框架和Web服务
date: 2017-06-08 11:26:05
tags: 重点
comments: true
categories: Web

---
刚开始学习的Flask的时候, 看到很多文档上都在说Flask是个Web应用框架, 但是还不太明白整个Web框架如何工作, 以及它与Web服务器是什么关系, 今天我来顺理一遍.
首先来看一张很经典的图, Web服务器通过WSGI(Web Server Gateway Interface)与Web应用框架或者Web应用程序进行信息交换.
![](https://martin-upload.b0.upaiyun.com/web/2017/06/530c0c2893cfe1b426a08906ee5bc17e.png)

### Web服务器
Web服务器是运行在物理服务器上的一个被动程序, 当它在接收到客户端(主要是浏览器)发送的请求之后, 生成相应的响应返回给客户端.
因为服务器使用http协议与客户端进行信息交流, 所以Web服务器又被称作HTTP服务器.
![](https://martin-upload.b0.upaiyun.com/web/2017/06/40e5f673eea3f9317cbae0cfb357bfc9.png)

#### Web服务器工作原理

* 建立连接: 客户机通过TCP/IP协议建立到服务器的TCP连接.
* 请求过程: 客户端向服务器发送HTTP协议请求包, 请求服务器里的资源文档.
* 应答过程: 服务器向客户机发送HTTP协议应答包, 如果请求的资源包含有动态语言的内容, 那么服务器会调用动态语言的解释引擎负责处理"动态内容", 并将处理得到的数据返回给客户端. 由客户端解释HTML文档, 在客户端屏幕上渲染图形结果.
* 关闭连接: 客户机与服务器断开连接.

> 所有的请求和响应都要遵循[HTTP协议](https://github.com/xuelangZF/CS_Offer/blob/master/Network/HTTP.md)


### WSGI (Web Server Gateway Interface)
Python 里面的Web服务器网关接口, 其他程序也有类似的接口, 例如Java的Servlet API和Ruby的Rack(两个我都没有用过...)
WSGI的出现, 让开发者可以将Web框架与Web服务器的选择分隔开来, 不再相互限制

![](https://martin-upload.b0.upaiyun.com/web/2017/06/aaa672431431529a7d849f9a4f10540f.jpeg)

### Web框架 
早期开发站点确做了许多重复性劳动, 后来为了减少重复, 避免写出庞杂, 混乱的代码, 人们将Web开发的关键性过程提取出来, 
开发出了各种Web框架. 有了框架, 就可以专注于编写清晰, 易维护的代码, 无需关心数据库连接之类的重复性工作.
Python 中比较经典的Web框架: Flask, Django, Tornado, Pyramid等
好多比较经典的Web框架都使用MVC架构, 如下图所示:

用户输入URL, 客户端发送请求, 控制器（Controller）首先会拿到请求, 然后用模型（Models）从数据库取出所有需要的数据, 进行必要的处理, 
将处理后的结果发送给视图（View), 视图利用获取到的数据, 进行渲染生成HtmlResponse返回给客户端.
![](https://martin-upload.b0.upaiyun.com/web/2017/06/c04fb5e8cd4cc7638e72b6bd332ca348.png)

#### Web应用程序
应用程序部分主要是处理一些业务上的逻辑, 读取或更新数据库, 根据不同的请求返回不同的响应
![](https://martin-upload.b0.upaiyun.com/web/2017/06/9ec5254ec0f3aa72ee24e91c4ddc1d92.png)

