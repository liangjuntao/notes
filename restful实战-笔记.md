##简介##
**中心目标**  
1. 展示web基础技术--http应用协议、url命名标准、xml标记语言、使用场合及局限性。  
2. web的一套设计原则。Representational State Transfer，表示性状态转移，简称rest。  
3. 面向资源架构 Resource-Oriented Architecture，简称ROA    

**简单的web**  
1. 原来http 0.9协议，如此单调。。。  
http特别适合分布式internet应用，因为它没有值的一提的特性。你要什么，我便给你什么。缺点转化为优势，简单性便是他最大的能力。  
2. http 0.9看到：可寻址性，无状态性  
3. web依赖的两项技术：url，html和xml  

**大web服务**
1. wsdl，soap  
2. 大web服务效果：http成为传输庞大xml负载的协议，真正的描述信息在xml中，比如soap。  
3. 适应性，可扩展性，可维护性意味着出错点越多，以此忽略了http 0.9，的简单性。辩证法啊！！！  
4. ROA定义为一种替代RPC式（soap+wsdl）的简单方案。  
5. rpc暴露的是算法，这种服务，每个接口都不同；roa暴露的是内部数据，接口是统一的**？？？**  

##第一章 可编程的web##

**分类**
1. 不采用http，就不是基于web的。绝对正确！！

**方法信息**  
1.对数据采取什么样的操作，成为方法信息。其实就是put，get，post，delete，head等http请求的几种方法  
2. soap服务的方法信息是放在http消息体中的，这个很直观；  
3. 观察google的搜索，发出的是get请求，这个方法信息是放在请求行中的，不在请求头和消息体中！  

**作用域信息**  
这段比较有意思：  
采用什么样的服务设计，决定了哪些信息属于方法信息，哪些信息属于作用域信息。  
两个uri：  
`http://flickr.com/photos/tags/penguin`  
`http://api.flickr.com/services/rest/?method=flickr.photo.serarch&tag=pengunin`  

第一个uri：方法信息是获取（get），作用域信息是具有penguin的标签的照片  
第二个uri：方法信息是搜索照片，作用域信息是具有penguin的标签  
技术上看，两者没有区别，他们的http请求都是get请求。  
仔细揣摩`method=flickr.photo.serarch`这个方法使得http本身的GET方法失去了本意！！！  
这两者架构上是有区别的！！！  
感觉好像是有那么点不同，但是说不上来！！！  

**rest式，面向资源的的架构**  
1. rest式架构意味着：方法信息都在HTTP方法中；roa架构意味着，作用域信息都在uri中。一个面向资源的rest式web服务，通过http请求行就可以了解用户要做什么了。  
`GET /reports/open-bugs HTTP/1.1`  
简单命令，我就是要获取这个uri中表示的资源！！  
如果作用域信息不放在uri中，那么服务就不是面向资源的！  
2. 一些知名的rest式面向资源的web服务：  
--雅虎的web服务  
--web应用，尤其是搜索引擎这种只读的服务  

**Rpc式架构**  
1. rpc式架构意味着：方法信息和作用域信息都在信封或报头，即消息体和消息头中；soap是典型的rpc式架构。    
2. 各个rpc服务采用自己的词汇，各个方法都不一样；而rest式web服务都一样，采用HTTP公用的方法；  
3. rpc式服务只暴露一个uri，具体的方法信息和作用域放在了消息体中传送，并且只支持一种HTTP方法-POST；但是，rest式服务就不一样了，它为不同的的作用域暴露不一样的uri，它发出的HTTP请求中不包含消息体，消息体是空的！！    
4. 只采用post方法的服务，多半是rpc服务。不绝对！

**混合架构**  
`http://api.flickr.com/services/rest/?method=flickr.photo.serarch&tag=pengunin`  
这个请求，方法信息是search操作，作用域是带penguin；  
方法信息搜索照片放在uri中，对于rest服务，方法信息应该放在http方法里，其余部分全部作为作用域信息。    
作用域信息在uri中，比较像面向资源的服务。  
所以，这是个rpc式的服务，鉴定完毕！！  
```    
GET services/rest?api_key=xxx&method=flickr.photo.serarch&tag=pengunin HTTP/1.1
```  
且看这个请求，大眼看上去，采用了get请求，作用域也在uri中，但这只是一个错觉。  
这个是rest-rpc混合服务。  


**http**  
1. 所有的web都采用http，不过使用方式不一样。  
2. rest式web，它的方法信息就是http方法中的某一个，作用域信息在uri中。  
3. rpc式web，方法信息和作用域信息在消息头和消息体中；它常把http作为容纳文档的信封，甚至是容纳另一种信封的信封（soap）    


**uri**  
1. 所有的web服务都用uri，不过有差异。  
2. rest式面向资源的服务会为客户端可能操作的每一则数据暴露一个uri。  
3. rest-rpc混合式，为客户端进行的每一个操作，暴露一个uri（如删除数据用一个uri，获取数据用一个uri）。  
4. rpc式，为每一个远程调用的进程暴露一个uri，一般来说这样的uri只有一个，即endpoint。其中，调用的方法和参数等信息，放入信封中信封中，即放入http消息体中。  

**soap**  
1. 基本上每个采用soap的web服务都属于rpc式架构。  
2. 真正的问题是：放在http信封的实体可以包含任何数据，soap信封中含有的xml数据是对一个rpc调用的描述。  

**wsdl**  
wsdl-web服务描述语言，其实是描述soap web服务的xml语法规则。通过wsdl文档，可以了解有哪些rpc方法，参数，返回值。   

**wadl**
wasl：web应用服务描述语言，描述rest式web服务的xml词汇。 


##第二章 编写web服务客户端##






