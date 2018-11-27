### HTTP首部

> HTTP协议的请求和响应报文中必定包含HTTP首部，主要讲解HTTP首部的结构以及各个字段的用法

* 6.1 HTTP报文首部
  - 报文首部 + 空行 + 报文主体
  - 请求报文首部组成： 方法 + URI + HTTP版本 + HTTP首部字段 
  - 响应报文是由HTTP版本 + 状态码 + HTTP首部字段
* 6.2 HTTP首部字段
  - 6.2.1 HTTP首部字段传递重要的信息
    * 起到传递重要信息的作用
    * 给浏览器和服务器提供主体报文的大小，使用的语言以及认证信息等
    * Content-Type ： text/html    表示报文主体的对象类型
    * 当然也可具有多个字段值 Keep-Alive : timeout=15, max = 100
  - 6.2.3 4种HTTP首部字段类型
    * 通用首部字段
    * 请求首部字段
    * 响应首部字段
    * 实体首部字段
  - 6.2.5 非HTTP/1.1 首部字段
    * 
* 6.3 HTTP/1.1 通用首部字段
  - 指的是请求报文和响应报文都会使用的首部
  - 6.3.1 Cache-Control  用来操作缓存的工作机制
    * 多个指令之间使用逗号分隔开，比如  Cache-Control : private  max-age=10, no-ache
    * 其中第一个指令代表的是，响应只能以特定的用户对位对象，这与public相反
    * 缓存只会对这个特定的用户提供资源缓存，对于其他的请求不会返回缓存
    * no-cache  指令的目的是为了防止从缓存中返回过期的资源
    * no-store  真正意义上的不缓存
    * s-manange 与max-age指令作用相同，适用于向多位用户提供缓存的公共服务器，一般指的是代理
    * max-age 如果值大于0时，直接取到缓存中的数据，否则重定向到服务器取出
    * min-fresh 指定资源过了这个时间都没有办法作为响应返回
    * max-stale 即使资源过期，但是只要不超过此值设置的数值就会返回
    * only-if-cached  不回去重新请求或者不确认资源的有效性就返回
    * must-realidate 代理会重新去验证此次返回资源的有效性，会忽略max-stale的作用
    * proxy-revalidate  代理每次返回缓存时，必须验证缓存的有效性
    * no-transform  指定缓存不可作为压缩的
  - 6.3.2 Connetion
    * 主要有2点; 控制不给代理转发的首部字段；保持持久连接；
    * 首部字段需要在conection中添加keep-alive指令来保证持久的连接
  - 6.3.3 Date 表示http报文创建的时间
  - 6.3.4 Pragma  是保留字段，为了兼容1.0版本
  - 6.3.5 Trailer  表明在报文主体后有主要的属性信息声明
  - 6.3.6 transfer-Encoding  表明传输报文主体时使用的编码格式
  - 6.3.7 upgrade  用于检测http协议以及其他协议是否可使用更高的版本进行通信，
* 6.4 请求首部字段
  - Accept 指明客户端能够处理的媒体类型 ，可指定权重   q=0.3
  - Accept-charset  表明用户端可以处理的字符集格式，同时也具有权重
  - Accept-Encoding  表明客户端可以支持的编码格式，如gzip, compress,deflafe
  - accept-language 表明客户端可以支持自然语言集，可有权重
  - Authorization 表明页面需要http验证信息，证书值传给服务端
  - Except  表明客户端希望出现某种特定的腥味，如果没有办法理解，服务端会出现417
  - From  告诉服务器用户代理电子邮件地址
  - Host  告诉服务器所请求的资源在互联网的某一个IP和端口
  - If-Watch  具有前缀If的请求首部字段，可以成为条件请求，服务器在接受后，如果判定条件为真时，才会执行
  - User-agent  用于传达浏览器的种类
* 6.5 响应首部字段
  - Accept-Range  表示客户端是否可以处理范围请求来获得服务端某个部分的请求，一种是bytes 或者为none
  - Age  表明原服务器在多久前创建的响应，单位为秒
  - Etag  
  - location 指明重定向的URI
  - Proxy-Authenticate  代理服务器所需要的认证信息发送给客户端
  - 