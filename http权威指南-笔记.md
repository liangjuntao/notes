**Web 服务器会为所有 HTTP 对象数据附加一个 MIME 类型**  
Content-type: image/jpeg
Content-length: 12984  
其中image就是MIME类型  

**mime:**  
最初设计 MIME（Multipurpose Internet Mail Extension，多用途因特网邮件扩展),
是为了解决在不同的电子邮件系统之间搬移报文时存在的问题。 


**mime类型:**  
html --> text/html  
ascii --> text/plain  
jpeg --> image/jpeg  
gif --> image/gif


**uri:统一资源标识符。**
分为两种，一种是url，一种是urn。  
url：统一资源定位符。  

**url格式：**  
schema ip地址 资源

**状态码：**  
200-成功，302-冲定性，404-资源找不到。

**http报文格式**   
起始行  
首部字段  
主体  

**Content-Type**   
首部说明了文档的 MIME 类型。  

**http网络协议栈**
http  
tcp  
ip  
联络接口  
物理网络硬件  


**http请求流程：**
(a) 浏览器从 URL 中解析出服务器的主机名；  
(b) 浏览器将服务器的主机名转换成服务器的 IP 地址；  
(c) 浏览器将端口号（如果有的话） 从 URL 中解析出来；  
(d) 浏览器建立一条与 Web 服务器的 TCP 连接；  
(e) 浏览器向服务器发送一条 HTTP 请求报文；  
(f) 服务器向浏览器回送一条 HTTP 响应报文；  
(g) 关闭连接， 浏览器显示文档。   
  
**telent程序是基于tcp/ip进行纯文本传输。**

小程序：  
telnet netcat  

**web结构组件；**
1. 代理  
2. 缓存  
3. 网关：用于将 HTTP 流量转换成其他的协议。  
如：一个 HTTP/FTP 网关会通过 HTTP 请求接收对 FTP URI 的请求，   但通过 FTP协议来获取文档
4. 隧道：对 HTTP 通信报文进行盲转发的特殊代理  
如：HTTP 隧道的一种常见用途是通过 HTTP 连接承载加密的安全套接字层（SSL，Secure Sockets Layer） 流量， 这样 SSL 流量就可以穿过只允许 Web 流量通过的防火墙了。  
5. agent代理：发起自动 HTTP 请求的半智能 Web 客户端  
如：代表用户发起 HTTP 请求的客户端程序  

**uri= url + urn**  
大多数 URL 都有同样的：  
“方案 :// 服务器位置 / 路径” 结构  

**url语法：**  

    <scheme>://<user>:<password>@<host>:<port>/<path>;<params>?<query>#<frag>  

ftp://prep.ai.mit.edu/pub/gnu;type=d    
在这个例子中， 有一个参数 type=d， 参数名为 type，值为 d。   

http://www.joes-hardware.com/tools.html#drills    
片段 drills 引用了 Joe 的五金商店 Web 服务器上页面 /tools.html 中的一
个部分。 这部分的名字叫做 drills。  

http://www.joes-hardware.com/hammers;sale=false/index.html;graphics=true  
这个例子就有两个路径段， hammers 和 index.html。 hammers 路径段有参数 sale， 其值
为 false。 index.html 段有参数 graphics， 其值为 true。  

    ？ 查询
    # 片段
    ；参数分割


rtsp/rtspu:  
RTSP URL 是可以通过实时流传输协议（Real Time Streaming Protocol） 解析的音 / 视频
媒体资源的标识符。    
方案 rtspu 中的 u 表示它是使用 UDP 协议来获取资源的。
`rtsp://<user>:<password>@<host>:<port>/<path>`  
`rtspu://www.joes-hardware.com:554/interview/cto_video`  


    状态码：
    100 ～ 101 信息提示  
    200 ～ 206 成功  
    300 ～ 305 重定向  
    400 ～ 415 客户端错误  
    500 ～ 505 服务器错误  

401 Unauthorized（未授权） 需要输入用户名和密码

原因短语和状态码是成对出现的     
HTTP/1.0 200 OK 中， OK 就是原因短语  


**消息头部：**  
Date:Tue,3Oct 1997 02:16:03 GMT     
--服务器产生响应的日期  
Content-length:15040     
--实体的主体部分包含了 15040 字节的数据  
Content-type:image/gif 
--实体的主体部分是一个 GIF 图片  
Accept: image/gif, image/jpeg, text/html   
--客户端可以接收 GIF 图片和 JPEG 图片以及 HTML  


HEAD方法只访问头部，不会受到消息体；  

TRACE方法主要用于诊断；   
也就是说， 用于验证请求是否如愿穿过了请求 / 响应
链。 它也是一种很好的工具， 可以用来查看代理和其他应用程序对用户请求所产生
效果


扩展方法；
LOCK 允许用户“锁定” 资源——比如， 可以在编辑某个资源的时候将其锁定， 
以防别人同时对其进行修改  
MKCOL 允许用户创建资源  
COPY 便于在服务器上复制资源  
MOVE 在服务器上移动资源  


状态码：100 Continue
100是一种优化；
比如我要向服务器发送一个大实体，
客户端在向服务器发送一个实体之前，并且等待服务器的100 Continue响应；那么可以发送100 Continue的Expect首部；
但是可能会有超时，超时客户端会直接发送大实体，不等待100 Continue响应；


200 OK 
请求处理，消息体重包含所访问的资源。


201 Created
用于创建服务器对象的请求。
并将整个对象放在消息体中响应。

202 Accepted
请求已经接收，但服务器未执行任何动作。
不能保证服务器会完成这个请求。

204 No Content
响应中没有消息体。

205 Reset Content
告知浏览器清楚当前页面中所有表单元素

**300~399 重定向状态码**  
作用；  
1.告知客户端使用替代位置去访问资源  
2.提供一个替代的响应？？？什么意思？？  

300 Multiple Choices  
意思是请求的url有多个版本时，返回这个状态码，提供一个可选列表，告知客户端去选择。
即一个沟通的过程。  
客户端请求一个包含多个资源的url时，返回这个状态码。  
基本上都是有一个Location字段提供访问资源。  

301 Moved Permanently：   
永久撤离的资源  
资源位置已经不在了，提供一个新位置给客户端。  
消息头中会有Location字段  
Location：host：path  

302 Found  
同301，资源不在了。消息头会有Location字段。  
区别：将来的请求仍应使用老的 URL ????没看懂。。。  

303 See Other  
临时撤离的资源；Url增强，重写url；负载均衡；服务器关联；  
post请求资源不在了，Location字段，将请求定向到这个资源上。  

304 Not Modified  
If-Modified-Since: Fri, Oct 3 1997 02:16:00 GMT  
返回304，告知这个时间点之后都没有被修改过，无消息体  

 
**400～499——客户端错误状态码**  

400 Bad Request  
错误的请求    
 
401 Unauthorized  
未授权，需要授权  

403 Forbidden     
用于说明请求被服务器拒绝了。 如果服务器想说明为什么拒绝请  
求， 可以包含实体的主体部分来对原因进行描述。 但这个状态码通  
常是在服务器不想说明拒绝原因的时候使用的 

404  
资源未找到  

405 Method Not Allowed  
方法不支持  
会有Allow字段来生命支持的方法。  


406 Not Acceptable  
通常是客户端请求的Acceptable什么类型的实体，服务器又不匹配这些资源。  


408 Request Timeout  
请求超时  

409 Conflic  
请求的资源可能引发冲突时，会采用这个  

410 Gone  
曾经拥有，告知现在不在了  
   
...太多了  
  


**通用首部：**  
Connection   
--允许客户端和服务器指定与请求 / 响应连接有关的选项  
Date5   
--提供日期和时间标志， 说明报文是什么时间创建的  
MIME-Version   
--给出了发送端使用的 MIME 版本  
Trailer  
--如果报文采用了分块传输编码（chunked transfer encoding） 方式， 就可  
以用这个首部列出位于报文拖挂（trailer） 部分的首部集合  
Transfer-Encoding   
--告知接收端为了保证报文的可靠传输， 对报文采用了什么编码方式  
Update   
--给出了发送端可能想要“升级” 使用的新版本或协议  
Via   
--显示了报文经过的中间节点（代理、 网关）  

**通用缓存首部：**  
Cache-Control   
--用于随报文传送缓存指示  
Pragma7   
--另一种随报文传送指示的方式， 但并不专用于缓存  


**Accept首部：**  
Accept   
--告诉服务器能够发送哪些媒体类型  
Accept-Charset   
--告诉服务器能够发送哪些字符集  
Accept-Encoding   
--告诉服务器能够发送哪些编码方式  
Accept-Language   
--告诉服务器能够发送哪些语言  

**条件请求首部：**  
Expect  
--允许客户端列出某请求所要求的服务器行为  
If-Match   
--如果实体标记与文档当前的实体标记相匹配， 就获取这份文档   
If-Modified-Since   
--除非在某个指定的日期之后资源被修改过， 否则就限制这个请求  
If-None-Match   
--如果提供的实体标记与当前文档的实体标记不相符，就获取文档  
If-Range   
--允许对文档的某个范围进行条件请求  
If-Unmodified-Since   
--除非在某个指定日期之后资源没有被修改过， 否则就限制这个请求  
Range   
--如果服务器支持范围请求， 就请求资源的指定范围  


**安全请求头部：**  
Authorization   
--包含了客户端提供给服务器， 以便对其自身进行认证的数据  
Cookie   
--客户端用它向服务器传送一个令牌——它并不是真正的安全首部， 但确实  
隐含了安全功能  
Cookie2   
--用来说明请求端支持的 cookie 版本  


**响应信息首部：**
Age   
--（从最初创建开始） 响应持续时间  
Public     
--服务器为其资源支持的请求方法列表  
Retry-After   
--如果资源不可用的话， 在此日期或时间重试  
Server   
--服务器应用程序软件的名称和版本  
Title  
--对 HTML 文档来说， 就是 HTML 文档的源端给出的标题  
Warning   
--比原因短语中更详细一些的警告报文  


**协商首部**  
如果资源有多种表示方法——比如， 如果服务器上有某文档的法语和德语译稿  
Accept-Ranges   
--对此资源来说， 服务器可接受的范围类型  
Vary   
--服务器查看的其他首部的列表， 可能会使响应发生变化； 也就是说，这是  
一个首部列表， 服务器会根据这些首部的内容挑选出最适合的资源版本发送给客户端    


**安全响应首部**

Proxy-Authenticate   
--来自代理的对客户端的质询列表  
Set-Cookie   
--不是真正的安全首部， 但隐含有安全功能； 可以在客户端设置一个令牌，  
以便服务器对客户端进行标识  
Set-Cookie2 与 Set-Cookie 类似， RFC 2965 Cookie 定义；  
参见 11.6.7 节 
WWW-Authenticate   
--来自服务器的对客户端的质询列表  


**实体首部：**  
Allow   
--列出了可以对此实体执行的请求方法  
Location   
--告知客户端实体实际上位于何处； 用于将接收端定向到资源的（可能是新  
的） 位置（URL） 上去  


**内容首部：**  
内容首部提供了与实体内容有关的特定信息， 说明了其类型、 尺寸以及处理它所需  
的其他有用信息。 比如， Web 浏览器可以通过查看返回的内容类型， 得知如何显示  
对象。  

Content-Base   
--解析主体中的相对 URL 时使用的基础 URL  
Content-Encoding   
--对主体执行的任意编码方式  
Content-Language   
--理解主体时最适宜使用的自然语言  
Content-Length   
--主体的长度或尺寸  
Content-Location   
--资源实际所处的位置  
Content-MD5   
--主体的 MD5 校验和  
Content-Range   
--在整个资源中此实体表示的字节范围  
Content-Type   
--这个主体的对象类型  



**实体缓存首部**
Expires   
--实体不再有效， 要从原始的源端再次获取此实体的日期和时间  
Last-Modified   
--这个实体最后一次被修改的日期和时间  

**套接字编程其实就是对tcp/ip的一次封装，提供接口来开发**  
s = socket(<parameters>)   
--创建一个新的、 未命名、 未关联的套接字  
bind(s,<local IP:port>)   
--向套接字赋一个本地端口号和接口  
connect(s, <remote IP:port>)   
--创建一条连接本地套接字与远程主机及端口的连接  
listen(s,...)   
--标识一个本地套接字， 使其可以合法接受连接  
s2 = accept(s)   
--等待某人建立一条到本地端口的连接  
n = read(s, buffer, n)   
--尝试从套接字向缓冲区读取 n 个字节  
n = write(s, buffer, n)   
--尝试从缓冲区中向套接字写入 n 个字节  
close(s)   
--完全关闭 TCP 连接  
shutdown(s,<side>)   
--只关闭 TCP 连接的输入或输出端  
getsockopt(s,...)   
--读取某个内部套接字配置选项的值  
setsockopt(s,...)   
--修改某个内部套接字配置选项的值  


**http事务时延的原因：**  
章节：4-2-1  
1.dns解析时间  
2.tcp三次握手  
3.网络传输请求报文  
4.回送http响应  


先大概过一遍


**持久链接：**  
实现 HTTP/1.0 keep-alive 连接的客户端可以通过包含 Connection: Keep-Alive  
首部请求将一条连接保持在打开状态  

Connection: Keep-Alive  
Keep-Alive: max=5, timeout=120  

这里有个 Keep-Alive 响应首部的例子，   
这个例子说明服务器最多还会为另外 5 个事务保持连接的打开状态，   
或者将打开状态保持到连接空闲了 2 分钟之后。  

如果客户端正在与一台 Web 服务器对话，   
客户端可以发送一个 Connection: Keep-Alive 首部来告知服务器它希望保持连接的活跃状态。   
如果服务器支持 keep-alive， 就回送一个 Connection: Keep-Alive首部， 否则就不回送  。  


**代理不服务要避免Connection: Keep-Alive**  
1.Connection: Keep-Alive  
到达最后服务器时，服务器会保持tcp连接，不会发出和代理的断开操作。    
2.客户端收到服务器的Connection: Keep-Alive之后，会在之前tcp连接上发送请求，但是代理不认为同一  个tcp连接会有新请求过来，所以不处理。这样客户端就在挂起了。。。  
妈卖批，跟谈恋爱似的。。。我以为你会接受我，原来只是个误会，不会啊。。。
 
**Content-Length：**      
作用之一就是保证tcp连接不会立马中断，而是在请求实体服务器都接受之后，或者客户端接受之后，才关闭tcp连接。  



**User-Agent**   
首部可以将用户所用浏览器的相关信息告知服务器， 包括程序的名称和版本， 通常还包含操作系统的相关信息。  


**cookie：**  
1.会话cookie  
2.持久cookie  

响应头：  
Set-cookie:  
pref=compact;   
domain="airtravelbargains.com";   
path=/autos/  


**cookie和缓存：**  
Cache-Control:no-cache="Set-Cookie"  
--除了 Set-Cookie 首部之外文档是可缓存的  

Cache-Control:public   
--可缓存文档  

小心处理cookie缓存；    
策略：对于带有cookie头部的图片可以进行缓存，一些私人化的文本信息，不采用缓存。  





