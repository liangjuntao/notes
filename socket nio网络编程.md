ServerSocket 服务端  
Socket 客户端  

**传统bio**  
传统的bio中，每一个客户端进行的连接的时候，server都会创建一个线程去处理。  
这样的做法有线程创建瓶颈，耗尽系统资源。  
  
**传统伪异步**  
jdk1.5之前没有nio时候，采用伪异步。    
将客户端的socket封装为一个task任务（实现runable），然后放入线程池中，配置响应的队列去实现。  
线程池+队列。  
线程池作用：讲socket链接放入到有界队列中；  

**异步非阻塞**  
jdk1.7之后真正实现异步非阻塞；
jdk1.5中实现了非阻塞，但并未实现异步。  
  
**bio和nio（jdk1.5）区别**  
本质区别是阻塞和非阻塞区别。  
阻塞概念：应用程序在获取网络数据到时候，如果网络数据传输很慢，那么应用程序将一直等待，直到传输完毕。  
非阻塞概念：应用程序直接获取已经准备好的数据，无需等待。    

阻塞底层原理：  
在tcp建立连接后，服务器端往客户端发数据分为两步： 
1先发送消息头，告知客户端我要给你传10个字节的数据。（但是网络不好，传到第六个字节的时候，网络卡了）  
2.客户端在接受到消息头的时候，就会一直等待服务器传10个字节的数据。到第六个字节，卡。那就会一直等待。  
  
非阻塞原理：  
1.服务器端讲数据准备好放入buff，准备好之后发送single给客户端，客户端就直接去取数据。  

bio：同步阻塞；  
nio：同步非阻塞；  
nio并没有实现异步，直到jdk1.7之后，才支持异步非阻塞。

**同步和异步**  
同步和异步是面向操作系统与应用程序对IO操作的层面上区别的。  
同步：应用程序直接参与IO读写操作，并且应用会直接阻塞到某一个方法上，直到数据准备就绪；或者采用轮训的方式去检测数据状态。  
异步：所有的io直接交给操作系统去完成，应用程序和io没有直接关系。当操作系统完成io操作后，会发信号给应用程序，应用程序直接去取数据即可。  

同步/异步说的是server服务器的执行方式。面向操作系统层面的。  
阻塞/非阻塞说的是具体的技术，接收数据的方式（io，nio）


**nio介绍**  
non block i/o :同步非阻塞
Buffer :缓冲区  
Channel:管道，通道  
Selector:选择器，多路复用器  

**BIO（io）**：采用点对点直连的tcp链接方式，client直接和server建立socket链接；  
**NIO**：传统的tcp点对点直连方式Socket进行了一层抽象，不是client直接发起socket链接。不是tcp直连的方式。
而是客户端将SocketChannel注册到服务器端的Selector中，Selctor（单线程死循环）去轮训通道，检测通道的状态，执行相关的操作。  
**SocketChannel的状态**：connet,accept,read,write。
  
**Buffer：**        
面向流的io中，可以将数据直接写入或读取到Stream对象中；在nio中，所有的数据都是用缓冲区处理的读写。  
缓冲区实质上是一个数组。  
flip() 将pos置为0；同时limit变为当前buff中存在的数据长度；


**Channel **  
网络数据通过Channel读取和写入，通道和流的不同之处在于通道是双向的。  
客户端：SocketChannel  
服务器：ServerSocketChannel
    
channel.read(bufferArray)：
反过来念buffer通过channel读取数据，也就是写入到buffer
read()方法按照buffer在数组中的顺序将从channel中读取的数据写入到buffer

channel.write(buffer);
反过来念，从buffer中取数据，写出到channel中。  


**Selector **  
核心：选择已经就绪任务的能力，选择判断注册到Secletor上通道的状态，从而进行读写。    
Selector不断轮询注册在其上的通道。 
如果某个通道发生读写操作，这个通道就处于就绪状态，会被Selector轮询出来。  
然后通过SelectKey取得就绪的Channel集合，从而继续IO操作。  

客户端通过key注册到Selector上。
要点：selector可以成千上万个Channel通道注册，没有上限；  
使用epoll代替的了传统的select实现；（epoll是linux上的技术），获取无限客户端。  
意味着只要一个线程负责Selector的轮询，就可以接入无限的客户端。



    NIO的本质是避免原始的TCP建立连接使用3次握手的操作，减小开销。


AIO中：  
进行通信的时候数据的读写采用的是Future模型，异步读写。  
把io操作交给系统去做，系统io操作完了再返回结果给我。



BIO NIO AIO总结： 
同步阻，同步非阻，异步非阻   
BIO: client----Socket 链接----> Server，每一个链接都会创建一个新的线程；  
NIO: client--- Selector----Server ，通过Selector轮询Channle来和server通信；
AIO: client----线程组--->Server,nio中注册轮询的功能交给线程组去处理；通过CompletionHandler对象去读写，其中读写IO是Future异步模型。