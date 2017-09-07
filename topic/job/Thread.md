## 基础 ##
线程拥有自己的内存空间，即是java内存模型中的java虚拟机栈  （这个是线程私有的，别的都是共有的）
## jvm内存模型 ##
堆 栈（本地栈，java虚拟机栈） 方法区 运行时常量池 程序计数器 直接内存

## Thread类 ##
**start方法**  
start调用native start方法.  
start()启动了一个线程之后，在线程获取cpu调度之后就自动进入了run()方法。  
实际上，start()方法的作用是通知 “线程规划器” 该线程已经准备就绪，  
以便让系统安排一个时间来调用其 run()方法，也就是使线程得到运行。  
	
**sleep方法**  
持有锁，目的是将cpu调度让给别人  
先对入参进行判断，设置了休眠的范围。  
	
**yield方法**  
native方法  
	
**join方法**  
调用wait()阻塞宿主线程，wait（0）释放锁，然后宿主线程获取锁。  
在a线程中调用b.join()，需要等b执行完毕继续执行a线程。  
	
## Object类 ##
wait() 释放锁  
notify()   唤醒wait的线程  
以上要和sync一起使用。

## 线程上下文切换 ##
存储恢复cpu状态的信息，使得线程可以从中断点继续执行。  


## 继承thread类和实现runable接口的区别和联系 ##
### 区别 ###
1. 实现runable接口为主，和继承thread类如下好处：
避免java单继承的局限性，一个类可以实现多个接口；
2. runable可以实现资源共享。  
一个继承Thread类中声明一个变量。  
若是采用继承thread，三个thread类.start(),启动时创建了三个实例对象，变量共享。  
若是实现接口，那么只会创建一个Thread类，变量就可以共享了。  
``` new Thread(thread).start() ```
``` new Thread(thread).start() ```
``` new Thread(thread).start() ```  
这三个操作共享了一个thread实例类，自然可以共享变量了。

### 联系 ###
thread 实现了runable 接口


## 实现Runnable接口相比继承Thread类有如下优势 ##
- 可以避免由于Java的单继承特性而带来的局限；
- 增强程序的健壮性，代码能够被多个线程共享，代码与数据是独立的；
- 适合多个相同程序代码的线程区处理同一资源的情况。
 

## 实现Runnable接口和实现Callable接口的区别 ##
- Runnable是自从java1.1就有了，而Callable是1.5之后才加上去的。 
- Callable规定的方法是call(),Runnable规定的方法是run()。  
- Callable的任务执行后可返回值，而Runnable的任务是不能返回值(是void)。  
- call方法可以抛出异常，run方法不可以
- 运行Callable任务可以拿到一个Future对象，表示异步计算的结果。它提供了检查计算是否完成的方法，以等待计算的完成，并检索计算的结果。通过Future对象可以了解任务执行情况，可取消任务的执行，还可获取执行结果。
- 加入线程池运行，Runnable使用ExecutorService的execute方法，Callable使用submit方法。

## 线程池基础 ##
### Executor接口 ###
接口定义：

	public interface Executor {
	    void execute(Runnable command);
	}


### ExecutorService接口 ###

	public interface ExecutorService extends Executor {
		void shutdown();
		//这里返回的是Future的实现类对象，可以获取执行的状态等
		Future<?> submit(Runnable task); 
	}


### Future接口  ###

	public interface Future<V> {
		//作业是否完成
		boolean isDone();
		
	}


## 线程池提交作业的几种方式及区别 ##

