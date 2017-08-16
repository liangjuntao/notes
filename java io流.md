适配器模式：  
	字节流和字符流的转换
比如：文件输入流嵌套在输入流读操作器中。注：reader是字符流  
```
InputStream inputStream = new FileInputStream("c:\\data\\input.txt");  
Reader reader = new InputStreamReader(inputStream);  

```
  
装饰器模式：
	比如一次读写一个字节是很慢的，可以采用缓冲流和字节流的组装，达到一次读写一大块数据的能力。  
这就是对于字节流的一次装饰，作用没有改变读写，但是读写的能力得到了提升，起到装饰作用。




FileChannel 从文件中读写数据。

DatagramChannel 能通过UDP读写网络中的数据。

SocketChannel 能通过TCP读写网络中的数据。

ServerSocketChannel可以监听新进来的TCP连接，像Web服务器那样。对每一个新进来的连接都会创建一个SocketChannel。


nio中buffer是读写模式可切换的；
调用flip()会将从写模式切换到读模式；

buffer的三个属性； 
capacity:作为一个内存块，Buffer有一个固定的大小值，也叫“capacity”.你只能往里写capacity个byte、long，char等类型。一旦Buffer满了，需要将其清空（通过读数据或者清除数据）才能继续写数据往里写数据。
position:
limit: