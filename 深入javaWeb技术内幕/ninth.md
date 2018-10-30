# 第九章 Servlet工作原理解析
> javaWeb技术是如何基于Servlet工作，以及Servlet容器是如何工作的，如何启动，以及其生命周期

* 9.1 从Servlet说起
  - Servlet容器：比如Tomcat jetty 等各有优劣，主要介绍Tomcat
  - 分为4个等级，Tomcat Container容器 Engine  Host（Servlet容器），真正的容器是Context
  - 9.1.1 Servlet启动的过程
    * 
  - 9.1.2 Web应用的初始化工作
    * 初始化是在ContextConfig的configureStart中实现的，主要是解析web.xml文件，这个文件是一个Web应用的入口
    * 
* 9.2 创建Servlet实例
    - 创建Servlet对象：如果Servlet的load-on-startup配置项大于0，那么context容器在启动的时候会被实例化
    - 初始化Servlet: 
* 9.3 Servlet体系结构
    - 与servlet主动关联的是：servletConfig、servletRequst、ServletReponse,第一个是在Servlet初始化时就传递了，后2个是在请求到达时调用Servlet传递过来的
    - 握手型交互模式：2个模块为了数据的交互会准备一个交易场景，此场景一直会跟随交易过程直到交易的结束，场景是由交易对象指定参数定制的
* 9.4 Servlet是如何工作的？
  - 用户从浏览器发送请求包含以下信息：http://hostname:port/contextpath/servletpath,其中的hostname与port是用来建立与服务器的TCP连接的，
  后边的URL才是决定选择与服务器中的那个子容器
  - 
* 9.5 Servlet中的Listener
  - 是基于观察者模式设计的，都继承于EventListener
* 9.6 filter如何工作?
  - 是web.xml中比较常用的配置项， 完成与Servlet一样的功能，提供一个FilterChain
  - init()
  - doFilter()
  - destroy()
* 9.7 Servlet中的url-pattern
  - 作用是：匹配一次请求是否会执行这个Servlet或者Filter
  - 