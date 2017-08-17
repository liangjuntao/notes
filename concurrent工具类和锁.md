CyclicBarrier: 
形象说法，所有的运动员都准备好了，才开始跑。
当所有的线程都准备好了，意味着三个线程均已经调用cyclicBarrier.await()方法，
此时run()中调用await()方法之后的代码继续执行。
场景：
计算。同一个点开始计算。

CountDownLacth:  
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

了解一下线程run（）方法调用局部变量的条件。
30min