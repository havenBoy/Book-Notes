### 深入理解Session和Cookie
> 二者的作用都是为了保持用户与后端服务器的交互状态，各自有各自的优点与缺陷

* 10.1 理解Cookie
  - 当用户通过HTTP协议访问一个服务器时，服务器会把一些键值对返回给客户端浏览器
  - 当初设计的初衷是记录用户在一段时间内访问Web服务器的行为路径
  - 由于HTTp是一种无状态的协议，在每一次发出请求都会带有第一次访问服务器设置的信息
  - 10.1.1 Cookie属性值
    * 当前的Cookie是有2个版本的，分别是“Set-Cookie” 和“Set-Cookie2” 
  - 10.1.2 Cookie如何工作
    * 
* 10.2 理解Session
  - 如果Cookie很多的话，增加了服务器与客户端的传输量，Session是解决这个问题的
  - 在客户端与服务器交互信息的时候会生成一个ID值，对于客户端是唯一的，
  - 10.2.1 Session与Cookie
    * 基于URl Path parameter  是默认支持的
    * 基于Cookie，如果没有默认修改Context的Cookie标识，默认是支持的
    * 基于SSL 默认不支持，设置为SSLEnable为True时才是支持的
