# 概述 #
技术范畴：分布式协调技术  
核心概念：分布式锁  
两种实现：googole的chubby， apache的zookeeper  

在zk提供分布式锁的基础上，
又提供了其他使用方法：配置维护，组服务，分布式消息队列，分布式通知/协调。  
zk采用的协议：zab  

# zk的数据模型 #
- 数据结构：Znode
- 原语：	关于该数据结构的一些操作
- 通知机制：watcher,服务是通过消息以网络的形式发送给分布式应用

## Znode ##
类似文件系统，zk树中的每个节点被称为Znode。
和文件系统的区别之处：  
1. 引用方式。绝对路径，唯一标识。有一些特殊字符串不能使用，如"/zookeeper"
2. znode结构。兼具文件和目录作用。文件维护数据，元信息，acl，时间戳。目录作为路径唯一标识。每个znode由三个部分组成：
 - stat：状态信息，描述znode版本，权限
 - data：znode关联的数据（配置信息，状态信息，汇集位置等kb单位的数据）
 - children：znode的子节点
3. 数据访问。每个节点存储的数据要被原子性操作。读操作将获取该znode所有数据，写也会替换该znode所有数据。每个节点还有acl，特定用户的权限列表。
4. 节点类型。
 - 临时节点。生命周期依赖于创建它的会话。session结束，节点被删除。不允许有临时节点。
 - 永久节点。不依赖于会话，只有显式被删除时，才会被删除。
5. 顺序节点。
6. 观察。对znode设置watch，称之为监视器。当znode发生改变时候（增删改）会触发watch对应的操作。触发时，zk向客户端仅且发送一条信息。watch是一次性消费，不会第二次消费这个watch。目的：减少网络流量。？？不太懂。。。

## zk中的时间 ##
1. zxid。对节点状态改变的操作都会使节点接收到一个时间戳，时间戳全局有序。zk维护三种zxid值。czid，mzid，pzid。c：创建。m：修改。64位：高32是epoch标识leader关系是否改变，每次一个leader被选出，都会有一个新的epoch。低32位是个递增计数。
2. 版本号
 - version：节点数据版本号
 - cversion：子节点版本号
 - aversion：节点所拥有acl版本号

## zk节点属性 ##
- czxid
- mzxid
- ctime
- mtiem
- version
- cversion
- aversion
- dataLength
- numChildren
- pzxid

## zk服务中的操作 ##
- create 创建znode
- delete 删除znode
- exists 测试znode是否存在，并获取元数据
- getAcl/setAcl 获取/设置acl
- getChildren 获取znode所有子节点列表
- getData/setData 获取/设置znode相关数据
- sync 使客户端的znode视图与zookpper同步

更新操作：  
- 必须明确指定版本号，可以调用exists找到当前版本号，如果版本号一致，更新成功；不一致，失败。
- 非阻塞式。如果客户端更新失败（另一个进程也在更新），不会阻塞进程继续执行。  

  
## 分布式锁的应用场景 ##
保证在一个集群中，只有一个节点是master；不会存在多master。

## 共享锁的原理 ##
共享锁在同一个进程中很容易实现，但是在跨进程或者在不同 Server 之间就不好实现了。
Zookeeper 却很容易实现这个功能，实现方式也是需要获得锁的 Server 创建一个 EPHEMERAL_SEQUENTIAL 目录节点，
然后调用 getChildren方法获取当前的目录节点列表中最小的目录节点是不是就是自己创建的目录节点，如果正是自己创建的，那么它就获得了这个锁，如果不是那么它就调用 exists(String path, boolean watch) 方法并监控 Zookeeper 上目录节点列表的变化，一直到自己创建的节点是列表中最小编号的目录节点，从而获得锁，释放锁很简单，只要删除前面它自己所创建的目录节点就行了。

# zk中的角色 #
