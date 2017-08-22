经典io/nio/aio中的问题：    
tcp读包写包问题，数据接收的大小，实际通信读取与应答处理逻辑。  

netty通信步骤：  
1.两个nio线程组，一个接受客户端连接，一个进行网络读写。  
2.ServerBootstrap对象，配置netty参数。  
3.实际处理数据的类ChannelInitalizer。  
4.绑定端口，执行同步阻塞方法等待服务端启动即可。  

21min 40s