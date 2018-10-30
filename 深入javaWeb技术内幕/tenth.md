### 深入理解Session和Cookie
> 二者的作用都是为了保持用户与后端服务器的交互状态，各自有各自的优点与缺陷

* 10.1 理解Cookie
  - 当用户通过HTTP协议访问一个服务器时，服务器会把一些键值对返回给客户端浏览器
  - 当初设计的初衷是记录用户在一段时间内访问Web服务器的行为路径
  - 由于HTTp是一种无状态的协议，在每一次发出请求都会带有第一次访问服务器设置的信息
  - 10.1.1 Cookie属性值
    * 当前的Cookie是有2个版本的，分别是“Set-Cookie” 和“Set-Cookie2” 
  - 10.1.2 Cookie如何工作
    * 每次创建Cookie是在一个以NAME为Set-Cookie的MimeHeaders,是一个，不是独立的
    * 请求某个URL路径时，浏览器会根据URL路径符合条件的Cookie放在Request请求头中传给服务器
  - 10.1.3 Cookie 的限制
    * 一般对Cookie的大小和数量的限制
    * IE9 50个域名  4095个字节
* 10.2 理解Session
  - 如果Cookie很多的话，增加了服务器与客户端的传输量，Session是解决这个问题的
  - 在客户端与服务器交互信息的时候会生成一个ID值，对于客户端是唯一的，
  - 10.2.1 Session与Cookie
    * 基于URl Path parameter  是默认支持的
    * 基于Cookie，如果没有默认修改Context的Cookie标识，默认是支持的
    * 基于SSL 默认不支持，设置为SSLEnable为True时才是支持的
  - 10.2.2 Session如何工作
    * 有SessionID服务器就可创建HTTPSession对象，第一次通过request.getSession()方法获取，如果没有获取到，那么创建一个新的对象，在session容器中保存，容器维持了session的生命周期，过期会被回收，服务器关闭时会序列化到磁盘，从而做到了状态的维持
* 10.3 Cookie安全问题
  - Cookie是把保存的数据通过HTTP协议的头部从客户端传递到服务器，又从服务器端回传到客户端，所有的数据是在浏览器中的，安全性相对较低
  - 相对于Session是比较安全的，数据时存储在服务器的，只是Cookie传递一个SessionID
* 分布式Session框架
  - 10.4.1 存在的问题
    * 使用Cookie可以很好的解决应用的分布式部署问题，但是安全性比较低
    * 客户端Cookie存储的限制，存储的大小限制
    * Cookie 管理的混乱，使得每个应用都要管理自己的Cookie
  - 10.4.2 可以解决的问题
    * 
  - 10.4.3 总体的实现思路
    * 需要一个服务订阅服务器，在应用启动的时可以订阅许多可写的Session与Cookie，这样可以操作和控制不同应用的值
    * 
