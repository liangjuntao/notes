## zookeeper伪分布式集群搭建 ##
1. 环境
 - zookeeper-3.4.10
 - java version "1.8.0_144"
 - centos 6.9

2. zk的配置  

- zk目录：  
/root/soft/zk


- myid目录：  
/root/data/zk/zk1/myid 
/root/data/zk/zk2/myid  
/root/data/zk/zk3/myid  

- 【conf/zk1.cfg】
```
dataDir=/root/data/zk/zk1/   
clientPort=2181   
server.1=192.168.43.128:2888:3888   
server.2=192.168.43.128:2889:3889   
server.3=192.168.43.128:2890:3890 
```
  
- 【conf/zk2.cfg】
```
dataDir=/root/data/zk/zk1/   
clientPort=2182   
server.1=192.168.43.128:2888:3888    
server.2=192.168.43.128:2889:3889    
server.3=192.168.43.128:2890:3890  
```
  
- 【conf/zk2.cfg】
```
dataDir=/root/data/zk/zk1/    
clientPort=2183    
server.1=192.168.43.128:2888:3888    
server.2=192.168.43.128:2889:3889    
server.3=192.168.43.128:2890:3890  
```

3. 环境变量配置
```
JAVA_HOME=/root/soft/jdk1.8.0_144  
PATH=$JAVA_HOME/bin:$PATH   
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar  

ZOOKEEPER_HOME=/root/soft/zk  
PATH=$ZOOKEEPER_HOME/bin:$PATH  

export JAVA_HOME  
export ZOOKEEPER_HOME  
export PATH  
export CLASSPATH  
```

以上便是所有的配置。



