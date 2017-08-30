###NoSql简介
- KV存储数据库：redis,Oracle BDB
- 列式数据库:hbase,Rlak
- 文档型数据库:mogoDb,CouchDB
- 图形数据库:Neo4j,InfoGrid,Infinite Grapph


###非关系型数据库特点  
- 模型简单
- 数据一致性要求不高
- 对数据库性能要求高


###Redis简介  
以k-v形式存储，基于内存。  
优点：高并发读写  
缺点：无法做复杂的关系模型  
扩展性：  
水平扩展：加机器    
垂直扩展：加硬盘  


###redis三种部署策略  
1. 主从
 - 1主-写，2从-读。
2. 哨兵：实现主从切换
 - 哨兵节点监控另外三台的状况，心跳检测。
 - 主节点挂了，选举一个新的主。挂的主再加进来变为从节点。
3. 集群
 - 多主多从。


###和memCache区别  
1. meycache倾向于单点，redis在于集群能力
2. redis是串行执行，memcache是多实列，并行。（同时插入10条数据）


###问  
1. 什么情况下比较慢，原因是？  
为了数据的高可靠，必须开启AOF；其实就是记日志。多线程并发写，写的性能会降低。  
如何解决：  
加机器，把写分开  
redis + ssdb  

2.怎么解决高并发的？  
lvs ngx 前台的入口  
ngx ngx1 ngx2 业务拆分，流量控制  
后端的瓶颈是数据库  
需要从前台分流 业务层 数据库层降压 综合考虑  

   
###数据类型
- string
- hash
- list 类似java queue
- set
- zset


###如何设计  
id | name     |  age  
<- - -  - - - - - ->     
1  | zhangsan | 16 

策略一：  
key : 对应一个json对象，所有属性；      
策略二：  
一个hash，对应一个对象；这样可以取单个属性；  
策略三：  
当做mq使用；  
  
###list
lpush 头部加 先进后出 栈  
rpush 尾部加 先进先出 队列  

lpop 头部取   
rpop 尾部取  

lrange key 0 -1  查看所有  

rpoplpush 尾部取删，头部添加  


###set
string类型的无序的集合。  
类似java中的list。  
提供取交集，并集操作。


###zset
加序列号，有序的。   
zadd zset1 1 val1  
zadd zset1 3 val3   
zrange zset1 0 -1 withscores  带索引  

场景：排名 rank  


###高级命令
- keys * 【查询所有的键】
- keys list* 【模糊匹配】
- setex key 10 value 【设置10s的过期时间】
- expire key 10 【设置10s过期时间】
- ttl key 【查看key剩余时间，什么时候过期】
- persist key 【取消过期设置】
- select 数字 【选择数据库，总共分16份，默认取第0份。可以做备份。0--4，5--9，10--14可以存储三份。】
- move key 数据库下标【移动key到别的数据库】
- rename key name 【重命名】
- echo 【打印】
- dbsize 【打印key总数】
- info 【打印redis信息】
- config get * 【获取所有配置项】
- flushdb 数字 【清空当前数据库】
- flushall 【清空所有数据库】


###redis的安全性 
配置文件：[redis.conf]
      
- requirepass redis 【配置项添加设置密码为redis】
- auth redis 【输入密码redis进入】
- redis-cli -a redis 【其他方式】  


###主从复制
#####配置[redis.conf]
```1. slaveof master <masterIP> <masterPort>```   
```2. masterauth <master-password>```

slave 不支持读，会报错。

aof + ssdb（处理并发写） 

###哨兵
1. 监控主从数据库是否正常。
2. 主数据库出现故障时候，选举从数据库为主。不存在单点故障。
#####配置[sentinel.conf]
在任一台从服务上配置。  
```sentinel monitor <masterName> <masterIP> <masterPort> <1投票选举次数> # 名称、ip、端口、投票选举次数  ```
```sentinel down-after-milliseconds <masterName> 5000 #默认1s检测一次，这里配置500ms ```
```sentinel failover-time <masterName> 180000  ```  
```sentinel parallel-syncs <masterName> 2 ```  
启动哨兵服务： ```redis-server sentinel.conf --sentinel & ```  
查看哨兵服务：```redis-cli -h  ip -p 26379 info Sentinel  ```  


###redis简单事务（一般不适用）
multi 打开事务    
exec 执行操纵  
discard 取消回滚  
支持不好。  
四次操作，第四次失败了。正常应该回滚，但是不会回滚。  
注：一般不采用redis的事务支持。  


###持久化机制  
1. snapshotting 快照  
2. append-only file (aof) 记日志

#####快照
每隔一段时间，做快照一次。一般设置为多久多少个key做修改，则快照。  
生产环境，不推荐使用。  

#####aof
每一个操作记录日志。
  
- always 收到命令立即写入硬盘，效率最低，但可以保证数据可靠性。  
- everysec 每秒写一次磁盘  
- no  性能最好，持久化没保证。  
注：一般采用always。  


###发布订阅 
	subscribe <channle> 订阅
	publish <channel> <content> 发布


###session共享实现  
其实session共享的目的：  
多个tomcat识别是一个用户访问多个tomcat的。  


###cas单点登录


###java中使用redis 
一致性hash。  
简单的就是海量数据快速定位数据在哪个槽/库。  
redis实现：   
    select * from tb_user where id = 3;


###架构设计  
技术 + 设计  

