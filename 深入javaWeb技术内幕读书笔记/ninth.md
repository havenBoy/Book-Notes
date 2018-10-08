# 第九章 Servlet工作原理解析
> javaWeb技术是如何基于Servlet工作，以及Servlet容器是如何工作的，如何启动，以及其生命周期

* 9.1 从Servlet说起
  - Servlet容器：比如Tomcat jetty 等各有优劣，主要介绍Tomcat
  - 分为4个等级，Tomcat Container容器 Engine  Host（Servlet容器），真正的容器是Context
  - 9.1.1 Servlet启动的过程
    * 