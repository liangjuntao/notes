**CyclicBarrier:** 
形象说法，所有的运动员都准备好了，才开始跑。
当所有的线程都准备好了，意味着三个线程均已经调用cyclicBarrier.await()方法，
此时run()中调用await()方法之后的代码继续执行。
场景：
计算。同一个点开始计算。



**CountDownLacth:  **
用于监听某些初始化操作，等初始化操作完毕后，通知主线程继续工作。
场景：
```
CountDownLacth countDown = new CountDownLatch(2);
```
意味着需要有2次的countDown.coutDown()操作，才能唤醒执行过countDown.await()的线程。
具体列子：
t1线程依赖于t2，t3的初始化操作；
在t1中执行countDown.await()进入阻塞等待状态， 
t2,t3初始化完毕后调用countDown.countDown()，这时会唤醒t1继续执行。

另一个列子：  
zk集群中，web端创建实列对象链接zk集群；
接下来直接操作zk集群添加文件，这时候可能会报错。
因为zk实列去链接需要时间，这时候可能还没有链接成功。
可以起一个线程去判断时候链接成功，如果成功countDown.coutDaown()，
这时候通知主线程继续执行。



**Callable和Future使用**
futrue适用于耗时很长的业务逻辑时，有效减小系统的响应时间。
代码在ssm-maven的example.async中。

线程池的submit方法和execute区别：  
1. submit可以传入实现Callable接口的实列对象；
2. submit有返回值，返回Future对象。而execute没有返回值。


**信号量**
semaphone  
如何去解决高并发？
1.网络段
2.服务器层面 
	ng做反向代理做负载均衡（配置文件，权重，3，6）  
解决高并发的时候最重要的是业务，不是技术！  
ng/lvs/haprox + 模块化业务系统，垂直系统。在业务上做最大程度的分流。
3.细粒度层面  
	使用信号量做限流。
	final Semaphore semp = new Semaphore(5);
	semp.acquire();
	semp.release();
代码层面就是5个信号量，如果创建20任务，只有获取信号量的线程，才可以执行。
执行完会后释放信号量，别的线程才可以去执行。
4.采用redis  
	在redis中做用户的访问限制策略。
    限制用户ip访问某个页面，一分钟不能大于60次。
	expired是redis的一个参数，过期时间；设置60s，60s之后去清楚。

秒杀系统设计思路：  
秒杀系统单独拉出来，和业务系统不挂钩。


**锁： ** 
重入锁：ReentrantLock 一定要释放锁。
读写锁：高并发读多写少的情况下，性能高于重入锁。

**重入锁和sync同步区别：**  
**sync**：多线程之间协调，sync需要用wait nofity进行配合。
	不是很灵活，需要锁当前对象或者自己new 一个Object进行加锁。
	sync锁的是对象的锁。  

**ReentrantLock**：配合的是condition使用的，主要是两个方法。  
	Condition condition = lock.newcondition();
	condition.await() -- condition.signal();``
	对应于sync中使用对象锁的lock.wait()--lock.notify(); 
condition.await()会释放锁；
condition.signal()不会释放锁；
sync中lock.wait()会释放锁；
sync中lock.notify()不释放锁。

Lock类灵活是因为一个lock可以创建多个Condition对象；
con1
con2
con1.await()  con1.singal()
con2.await()  con2.singal()  
根据不同的condition去唤醒不的condition；

其中关于公平锁和非公平锁：
`Lock lock = new ReentrantLock(boolean isFair);` //指的是锁创建的时候类型
默认是非公平；  
公平锁意思是：哪个代码先调用了，那就先上锁。 //性能低，因为要维护加锁的顺序  
非公平锁：按照cpu的调度。                   //性能高，不用维护。

**锁的嗅探：**
lock.tryLock(); 获取返回true，没获取false；
可以自己加逻辑，不用等待了。
 
**读写锁**
实现读写分离。
    ReentrantReaderWriterLock rwLock = new ReentrantReaderWriterLock();
    ReadLock readLock = rwLock.readLock();
    WriteLock writeLock = rwLock.writeLock();
读读共享；读写互斥；写写互斥；

**分布式锁：**  
利用zk实现分布式锁。
先到zk注册，用户访问的时候先到zk去检测看加锁的代码是否被访问了。


了解一下线程run（）方法调用局部变量的条件。
30min