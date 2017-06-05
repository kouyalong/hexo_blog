layout: http_layout
title: HTTP的一些零散的知识点
date: 2017-05-27 11:50:05
tags: 重点
comments: true
categories: Http

---

#### 请求方式

根据HTTP标准, HTTP请求可以使用多种请求方法.
HTTP1.0定义了三种请求方法: GET, POST 和 HEAD方法.
HTTP1.1新增了五种请求方法: OPTIONS, PUT, DELETE, TRACE 和 CONNECT 方法.

* Get

* Post

* Put

* Delete

* Trace

* Connect

* Options

* Head


#### [Session](http://baike.baidu.com/link?url=7iUfdIN-T5IrrqaGndSHxoVZqRzDs8Gp5aFyVBhD5tLQb7zD6kTAN3wdFQfpl0YCr1tysE_dE9p5trpOodPV0K) 和 [Cookie](http://baike.baidu.com/link?url=-U4M-O34JWPr-fvQ9Wh5D0V7eMvP-hsWueRcKFY3tnnkUVioYOpJ8gzMX4sgEYLGT9FBepwQG6D6MJtefeacoK)

* Session
    > 在计算机中, 尤其是在网络应用中, 称为"会话控制". session对象存储特定用户会话所需的属性及配置信息
    > session是用于保持状态的基于Web服务器的方法. session允许通过将对象存储在Web服务器的内存中在整个用户会话过程中保持任何对象

* Cookie
    > 指某些网站为了辨别用户身份, 进行session跟踪而储存在用户本地终端上的数据; 在计算机中是个存储在浏览器目录中的文本文件, 它可以包含有关用户的信息.

* 比较
    * cookie数据存放在客户的浏览器上, session数据放在服务器上
    * cookie不是很安全, 别人可以分析存放在本地的cookie并进行cookie欺骗, 如果主要考虑到安全应当使用session
    * session会在一定时间内保存在服务器上. 当访问增多, 会比较占用你服务器的性能, 如果主要考虑到减轻服务器性能方面, 应当使用cookie
    * 不同浏览器对cookie处理是不一样的, 如果客户端禁止cookie, 则cookie不能建立
    * 并且在客户端, 一个浏览器能创建的cookie数量最多为300个, 并且每个不能超过4KB, 每个Web站点能设置的cookie总数不能超过20个

#### [状态码](http://www.runoob.com/http/http-status-codes.html)

当浏览者访问一个网页时, 浏览者的浏览器会向网页所在服务器发出请求. 当浏览器接收并显示网页前, 
此网页所在的服务器会返回一个包含HTTP状态码的信息头（server header）用以响应浏览器的请求.
HTTP状态码的英文为HTTP Status Code.

* 1** -- 信息, 服务器收到请求, 需要请求者继续执行操作

* 2** -- 成功, 操作被成功接收并处理

* 3** -- 重定向, 需要进一步的操作以完成请求

* 4** -- 客户端错误, 请求包含语法错误或无法完成请求

* 5** -- 服务器错误, 服务器在处理请求的过程中发生了错误




