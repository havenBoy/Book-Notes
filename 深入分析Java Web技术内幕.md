### 深入分析Java Web技术内幕读书笔记

##### 第一章：深入介绍web请求过程

首先B/S架构的服务推动了互联网的发展，逐渐代替了以前的C/S架构的服务，

**优点**体现于1.浏览器的操作简单；2.基于http请求，已经开发有许多的服务器为基础，所以只需要注重逻辑代码的编写；

http请求是**无状态的短连接**通信方式，不能一直保持这个链接的处于活的状态，是为了保证请求的快速响应；

**输入URL地址->请求DNS域名转为Ip地址-> 定位到具体的服务器(负载均衡) ->  资源（可能来自于CDN）与数据（可能来自于数据库）的请求**

如何发起一个http请求 其本质是一个socket连接，同时可以使用httpclient与Linux中的curl模拟请求的发起

**熟悉常见的http请求头和响应头**

请求头  Accept-charset,Accept-encoding,Accept-language,Host,user-Agent,connection

响应头  Server content-type  content-language,content-encoding content-length,keep-alive

**常见的http状态码**    200 成功 300 重定向  400 请求不存在 500 服务器内部错误

**浏览器的缓存机制**：

  浏览器中Ctrl+F5可以请求无缓存的数据，，这个是因为在这种操作的请求头中添加了2个参数，Pragm:no-cache,Control:no-cache

**DNS域名解析过程**

1.查看缓存中是否已有解析过的缓存，有的话则使用之前以及解析过的；

2.查找操作系统中是否有这个域名，windows和Linux下的
