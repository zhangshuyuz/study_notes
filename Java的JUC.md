#  JUC

## 1. JUC介绍

1. JUC介绍

   > 1. 什么是JUC
   >
   >    JUC，java.util.concurrent包的缩写，该包下Java提供给我们很多并发编程的工具类
   >    
   > 2. 如何学习JUC
   > 
   >    案例驱动学习，JUC的实例，参见D:/JucCode/
   >    
   > 3. java.util.concurrent.atomic
   > 
   >    原子性的包，里面提供很多原子性的类
   >    
   > 4. java.util.concurrent.locks
   > 
   >    锁的包
   >    
## 2. 进程和线程回顾
2. 进程和线程回顾
    > 1. 进程/线程是什么
    > 
    >    进程：提供一个完成服务的应用程序
    >    
    >    线程：进程中提供各种具体服务的执行序列
    >    
    > 2. 线程的状态
    > 
    >    线程状态有六种：
    >    
    >    NEW -- 线程被创建
    >    
    >    RUNNABLE -- 线程准备就绪
    >    
    >    BLOKEL -- 线程阻塞
    >    
    >    WAITING -- 线程持续等待
    >    
    >    TIMED_WAITING -- 线程定时等待
    >    
    >    TERMINATED -- 线程终结
    >    
    > 3. wait/sleep的区别
    > 
    >    wait：线程释放锁资源，线程处于等待唤醒状态。wait的线程被唤醒，会去争夺锁，然后从等待的地方开始继续执行。
    >    
    >    sleep：线程不释放锁资源，线程处于等待唤醒状态。sleep的线程被唤醒，从等待的地方开始继续执行
    >    
    > 4. 并发和并行
    > 
    >    并行：多个设备同时执行某一件事情
    >    
    >    并发：宏观上并行，微观上多个设备按照同样的时间间隔交替执行
    >    
## 3. Lock接口
3. Lock接口
    > 1. 复习synchronized
    >
    >     1. 多线程编程模板--上
    >     
    >         1. 多线程编程编写顺序：定义资源类，定义资源类对资源的操作，最后定义线程调用资源类
    >         
    >         2.  要做到高内聚低耦合：资源类和资源类的操作必须高内聚在资源类里面，资源类和用户线程之间必须做到低耦合
    >         
    >     2. 买票程序再书写：一个售票员，卖出30张票
    >     
    > 2. ReentrantLock
    >
    >     1. 介绍
    >     
    >        ReentrantLock，Re前缀重新，entrant重入，Lock锁，ReentrantLock即为可重入锁
    >        
    >        可重入锁，就是该锁可以反复被多个线程使用
    >        
    >     2. ReentrantLock和Synchronized相比，好处是什么
    >     
    >        ReentrantLock提供了比synchronized更多的api，可以做更多复杂的操作
    >        
    >     3. 使用ReentrantLock的套路
    >     
    >         ```
    >         Lock lock = new ReentrantLock();
    >         lock.lock();
    >         try {
    >         	// 需要上锁的代码块
    >         } catch() {
    >         	// 如果需要解决异常，加上catch从句
    >         } finally {
    >             lock.unlock();
    >         }
    >         ```
    >         
## 4. Java8特性
4. Java8特性
    > 1. Lambda表达式
    > 
    > 2. Stream流
    > 
    > 3. 方法引用
    > 
    > 4. 接口中引入了默认方法、静态方法的概念
    > 
## 5. 线程间通信--上
5. 线程间通信--上
    > 1. 线程通信经典例题：生产者消费者问题例题
    >
    >    操作一个初始值为0的变量，实现一个线程对其+1，一个线程对其-1，交替10轮 
    >    
    > 2. 线程间通信
    >
    >     1. 生产者和消费者
    >     
    >     2. 通知等待唤醒机制
    >     
    > 3. 多线程编程模板--中。生产者消费者问题
    >
    >    1、判断，然后wait()
    >    
    >    2、干活
    >    
    >    3、通知，然后notify()
    >    
    > 4. 多线程编程模板--下。**一定要注意线程之间虚假唤醒问题**
    >
    >    需要进行线程通信的线程，超过2个，则要考虑虚假唤醒问题上
    >    
    >    线程间虚假唤醒问题：在判断时，如果我们使用if判断，只会判断一次，因此会存在虚假唤醒问题
    >    
    >    **虚假唤醒问题解决：只要将判断方式改为while循环判断，则可以解决** 
## 6. 线程间通信--下
6. 线程间通信--下
    > 1. 上述线程间通信，都是基于synchronized关键字进行的
    >
    > 2. Java8新的实现：
    >
    >     1. 对标实现
    >     
    >         1. synchronized和Lock锁是对标的，因此synchronized可以实现线程间通信，Lock锁也可以实现。
    >         
    >         2. Lock锁实现线程间通信
    >         
    >             1. 介绍
    >             
    >                Lock锁也可以实现线程间通信，它需要对象Condition的辅助
    >                
    >             2. Condition对象
    >             
    >                通过lock.newCondition()来获取
    >                
    >                它里面有对应的方法await()和signal()，可以完成wait和notify功能
    >                
    >             3. Lock锁实现线程间通信实例：
    >             
    >                 ```
    >                 final Lock lock = new ReentrantLock();
    >                 final Condition cd = lock.newCondition();
    >                 
    >                 public void demo() throws Exception {
    >                 	lock.lock(); // 上锁
    >                 	try {
    >                 		while (判断条件)
    >                 			cd.await();
    >                 		业务逻辑;
    >                 		cd.signal();
    >                 	} finally {
    >                 		lock.unlock(); // 解锁
    >                 	}
    >                 }
    >                 ```
    >                 
## 7. 线程间定制通信
7. 线程间定制通信
    > 1. 之前的线程通信，只是单纯的消费者和生产者之间来回切换。如果我们想要完成更加复杂的线程通信，之前的无法满足。
    > 2. 案例：有三个线程AA、BB、CC，它们分别打印A、B、C，要求：AA打印5次，BB打印10次，CC打印15次，继续AA打印5次，BB打印10次，CC打印15次
    > 3. 多个线程之间，有顺序的通信，synchronized关键字的线程通信无法做到，要使用Lock锁的线程通信。
    > 4. 多个线程顺序调用的实现思路：
    >    1.  必须要有一个标识位
    >    2. 必须有多个Condition
    >    3. 判断标识位
    >    4. 执行业务逻辑
    >    5. 修改标识位，然后通知下一个
    >    
## 8. 线程不安全
8. 线程不安全
    > 1. ArrayList集合，是线程不安全的。通过查看源码可以看到，没有让线程安全的锁等操作。通过实际验证，也可以得到这个结论
    >
    > 2. 线程不安全的ArrayList集合，如果多线程进行写操作，会报错```java.util.ConcurrentModificationException```
    >
    >     * ```java.util.ConcurrentModificationException```：并发修改异常，在进行并发修改(包括并发写、并发改)操作时出现异常
    >     
    > 3. 如何将ArrayList变成线程安全的？
    >
    >    1. 解决方案1：仅知道即可，禁止使用，因为Vector集合太老了
    >    
    >       我们知道，ArrayList集合是List接口的实现类，在List的其他实现类中，有没有和ArrayList有同样架构，并且线程安全的集合呢？
    >    
    >       有，**Vector集合是线程安全的，并且有和ArrayList相同的架构**。
    >    
    >       Vector集合通过查看源码可以看到，它的方法都是加了synchronized关键字的，因此它是线程安全的
    >       
    >    2. 解决方案2
    >    
    >       Collections工具类中，有各种synchronized前缀的静态方法，这些静态方法可以将线程不安全的集合变成线程安全的集合
    >       
    >    3. 解决方案3
    >    
    >       上述两种，都是将并发操作加锁了，因此效率不高
    >       
    >       JUC给我们提供了最为好的解决集合线程不安全的方式：写时复制技术
    >    
    > 4. 写时复制 
    >
    >     1. 介绍
    >        
    >        JUC提供写时复制的类CopyOnWriteArrayList
    >        
    >        CopyOnWriteArrayList类使用了写时复制技术，是线程安全的，并且效率很高
    >        
    >     2. 原理
    >     
    >        使用了写时复制，写操作线程操作数据时，会复制出一个副本，写操作线程操作副本，此时读操作线程读取老版本；当写操作完成，将读操作线程迁移到新副本上，新副本作为最新的结果。 
    >        
    >     3. 使用
    >     
    >        CopyOnWriteArrayList也是List的实现类，因此直接使用多态写法，直接使用即可，例如：
    >        
    >         ```
    >         List<String> list = new CopyOnWriteArrayList<>();
    >         ```
    >        
    >      4. 扩展
    >     
    >         Set集合本身也是线程不安全的，我们也可以使用JUC提供的基于写时复制技术的线程安全的Set
    >         
    >         CopyOnWriteArraySet是JUC提供的写时复制的Set
    >         
    >      5. 再次扩展
    >     
    >         Map集合也是线程不安全的，但是JUC没有基于写时复制的线程安全的Map集合。但是JUC有没有提供线程安全的Map呢？
    >         
    >         有， ，它是线程安全的
    >         
    >      6. SynchronizedHashMap和ConcurrentHashMap的对比
    >     
    >         ConcurrentHashMap比SynchronizedHashMap更好
    >         
    >         因为synchronized是同步锁，可以类比为数据库的表锁，在进行操作时整个Map都会被锁住，因此效率低下。
    >         
    >         ConcurrentHashMap的加锁类似于数据库的行锁，并发操作时只会锁住其中一块，因此效率高
    >         
## 9. 多线程锁--8锁问题
9. 多线程锁--8锁问题
    > 1. 假设有一个资源类Phone，资源类中有两个被synchronized修饰的操作sendEmail()和sendMessage()。两个线程，第一个调用sendEmail()，第二个调用sendMessage()。根据这个来回答8锁问题。
    > 
    > 2. 8锁问题代码参见D:/JucCode，com.yuu.juc.threadlocks
    >   
    > 3. 8锁问题：
    > 
    >     1. 问：标准访问，哪个先被打印。
    >     
    >        Email先被打印
    >        
    >     2. 问：如果停留4秒在sendEmail()中。请问先打印Email还是短信
    >     
    >        Email先被打印
    >        
    >         * 注意：如果想要sleep()，可以使用TimeUtil来执行，因为它有更多关于时间的单位，比使用Thread调用sleep()好
    >        
    >     3. 问：第二个题的条件仍然存在。如果在资源类中新增一个方法Hello()，并且Hello()是个普通方法，第一个线程调用sendEmail()，第二个线程调用Hello()，问那个先被打印。
    >     
    >        先打印Hello，World
    >        
    >     4. 问：第二题条件仍存在。如果有两个资源类实例，两个线程分别用不同的实例，第一个线程调用sendEmail()，第二个调用sendMessage()。请问哪个先被打印。
    >     
    >        先打印Message
    >        
    >     5. 问：第二题条件仍存在。如果资源类的sendEmail()和sendMessage()是静态同步方法，一个资源类实例，第一个线程调用sendEmail()，第二个调用sendMessage()。请问哪个先被打印。
    >     
    >        先打印Email
    >        
    >     6. 问：第二题条件仍然存在。如果资源类的sendEmail()和sendMessage()是静态同步方法，两个资源类实例，分给两个线程。第一个线程调用sendEmail()，第二个调用sendMessage()。请问哪个先被打印。
    >     
    >        先打印Email
    >        
    >     7. 问：第二题条件仍然存在。如果sendMail()为静态同步方法，sendMessage()为同步方法，一个资源类。第一个线程调用sendMail()，第二个调用sendMessage()，请问那个先被打印
    >     
    >        先打印Message
    >        
    >     8. 问：第二题条件仍存在。如果如果sendMail()为静态同步方法，sendMessage()为同步方法，两个资源类，分给两个线程。第一个线程调用sendMail()，第二个调用sendMessage()，请问那个先被打印
    >     
    >        先打印Message
    >        
## 10. Callable接口
10. Callable接口
    > 1. 面试题：获取多线程的方式有几种？(一般这种几种问题不要直接回答几种，建议回答具体的方式)
    >
    >    一共有以下的方式：继承Thread类、实现Runnable接口、实现Callable接口、Java线程池获取
    >    
    > 2. Callable接口和Runnable接口的区别
    >
    >     1. Callable接口需要指定泛型   
    >     
    >     2. Callable接口重写的是call()方法，并且call()方法有返回值，返回值类型为泛型指定的数据类型
    >     
    >     3. Callable接口重写的call()方法，会抛出异常
    >     
    > 3. Callable接口和Runnable接口的相同点
    >
    >    Callable接口也是一个函数式接口，可以用Lambda表达式做匿名实现
    >    
    > 4. Callable接口使用
    >
    >     1. 可以直接写一个匿名实现类，直接使用在Thread的构造参数上吗？
    >
    >        无法直接用在Thread的构造方法参数上，因为Thread根本没有含Callable参数的构造方法
    >        
    >        我们知道，Thread的构造方法参数上，可以使用Runnable接口的实现类作为参数。Java提供了很多的Runnable接口的实现类。
    >       
    >        如果有个Runnable接口的实现类，不仅实现了Runnable接口，并且还实现了Callable接口或者可以接收Callable接口的实现类，则我们不就可以在使用Callable用在线程中了
    >        
    >     2. FutureTask类
    >     
    >        FutureTask类实现了Runnable接口，并且它的构造参数就只有一个Callable接口。它就是我们需要的
    >     
    > 5. FutureTask详解
    >
    >     1. 介绍
    >     
    >        FutureTask，未来任务
    >        
    >        FutureTask可以参照Future—Listner机制理解，理解为是多线程的异步线程模型
    >        
    >        我们可以将一些耗时任务交给其他线程去做，不用让主线程一直阻塞住
    >        
    >        FutureTask中有get()方法，获取任务执行后返回的结果
    >        
    >     2. FutureTask方法
    >     
    >         ```
    >         T get() -- 获取任务执行完成后的结果。如果任务没有完成，则get()方法会一直被阻塞住，直到任务完成
    >         
    >         boolean isDone() -- 判断该任务是否计算完毕
    >         ```
    >         
    >     3. FutureTask使用案例
    >     
    >         ```
    >         public static void main(String[] args) {
    >             FutureTask<String> ft = new FutureTask<>(()->{
    >             	return "耗时的任务";
    >             });   
    >             new Thread(ft, "Thread01").start();
    >             System.out.println("其他轻量任务");
    >             while (true) {
    >             	if (ft.isDone()) {
    >             		String msg = ft.get();
    >             		System.out.println(msg);
    >             		break;
    >             	}
    >             }
    >         }
    >         ```
    >     
    >     4. FutureTask执行的一般是复杂、耗时的任务，因此我们一般在最后才会使用get()方法获取任务结果，避免被阻塞(因为使用FutureTask就是不想被阻塞)
    >     
    >     5. FutureTask执行的是复杂任务，因此它自己有一套机制，如果已经传入一个线程，哪怕继续传给别的线程，也不会再次执行。**一个FutureTask实例的任务只会执行一次，如果想要多次执行一个任务，必须使用多个FutureTask实例**
    >     
## 11. JUC强大的工具类：只讲部分重点的
### 1. CountDownLatch
1. CountDownLatch
    > 1. 介绍
    >
    >    CountDownLatch，减少计数器
    >    
    > 2. 应用场景
    >
    >    例如，有一个主线程，它需要开启6个子线程，当全部子线程结束任务时，主线程才结束任务。如何实现？
    >    
    >    我们可能会想到使用sleep()方法，让主线程休眠来完成，但是休眠时间无法确定，因此不可以
    >    
    >    这里就可以使用CountDownLatch来做计数
    >    
    >     ```
    >    public static void main(String[] args) throws InterruptedException {
    >    
    >            CountDownLatch count = new CountDownLatch(6); // 初始化计数器
    >    
    >            for (int i = 0; i < 6; i++) {
    >                new Thread(()->{
    >                    System.out.println(Thread.currentThread().getName() + "结束任务");
    >                    count.countDown(); // 子线程任务完成，计数器减一
    >                }, String.valueOf(i)).start();
    >            }
    >    
    >            count.await(); // 让主线程等待，等待到计数器减为0，放行
    >    
    >            System.out.println("主线程结束任务");
    >            
    >        }
    >     ```
    >     * 我们其实还有一种方式，不使用CountDownLatch也可以实现。
    >    
    >       因为JVM默认会启动两个线程，一个main线程，一个GC线程。因此如果活跃线程数大于2，说明我们开启的线程还没有运行完毕
    >       
    >       我们可以使用一个while循环判断活跃线程数，如果活跃线程数大于2，则让main线程暂停
    >       
    >        ```
    >       while (Thread.activeCount() > 2) {
    >       	Thread.yield();
    >       }
    >       自己启动的线程执行完毕后，main线程要执行的逻辑代码;
    >        ```
    >    
### 2. CyclicBarrier
2. CyclicBarrier
    > 1. 介绍
    >
    >    CyclicBarrier，循环栅栏。
    >    
    >    用来让一组线程，在固定的同步点阻塞住，等全部都到达阻塞点后，继续一起执行某个事情 
    >    
    > 2. 应用实例
    >
    >     ```
    >     public static void main(String[] args) {
    >     
    >             CyclicBarrier cyc = new CyclicBarrier(7, ()->{
    >                 System.out.println("线程一起，完成一件共同的事情");
    >             }); // 指定要进行同步的线程数，指定同步后一起做的任务
    >     
    >             for (int i = 0; i < 7; i++) {
    >                 new Thread(()->{
    >                     System.out.println(Thread.currentThread().getName() + "完成自己的任务");
    >                     try {
    >                         cyc.await(); // 线程自己的任务完成，进行同步阻塞，等待其他线程
    >                     } catch (InterruptedException e) {
    >                         e.printStackTrace();
    >                     } catch (BrokenBarrierException e) {
    >                         e.printStackTrace();
    >                     }
    >                 }, String.valueOf(i)).start();
    >             }
    >     
    >         }
    >     ```
    >     
### 3. Semaphore
3. Semaphore
    > 1. 介绍
    >
    >    Semaphore，信号灯
    >    
    >    可以用来标记一些公共资源，是否有空闲的可以被占用；如果使用完资源，则标记资源重新可用
    >    
    > 2. 应用实例
    >
    >    例如，停车场车位和停车场汽车的关系。假如停车场有3个车位，有6辆汽车想要停车
    >    
    >     ```
    >    public static void main(String[] args) {
    >            Semaphore semaphore = new Semaphore(3); // 初始化信号量
    >    
    >            for (int i = 0; i < 6; i++) {
    >                new Thread(()->{
    >                    try {
    >                        semaphore.acquire(); // 争抢，信号量减少
    >                        System.out.println(Thread.currentThread().getName() + "争夺到车位，开进停车场");
    >                        Thread.sleep(3000);
    >                        System.out.println(Thread.currentThread().getName() + "开走汽车，离开停车场");
    >                    } catch (InterruptedException e) {
    >                        e.printStackTrace();
    >                    } finally {
    >                        semaphore.release(); // 释放，信号量增加
    >                    }
    >                }, String.valueOf(i)).start();
    >            }
    >        }
    >     ```
    >    
    > 3. 使用Semaphore的目的：
    > 
    >    1、公共资源的争夺使用
    >    
    >    2、并发线程数的控制
    >    
## 12. ReentrantReadWriteLock(读写锁)
12. ReentrantReadWriteLock(读写锁)
    > 1. 锁回顾
    >
    >     * 锁按照类别分类：
    >     
    >         * 乐观锁：Redis中的锁，是乐观锁
    >         
    >         * 悲观锁：所有关系型数据库的锁，都是悲观锁
    >         
    >     * MySQL数据库中的锁：
    >     
    >         * 行锁：InnoDB中使用行锁；行锁会出现死锁
    >         
    >         * 表锁：MyISAM中使用表锁；表锁不会出现死锁
    >     
    > 2. 读锁、写锁基本概念：
    >
    >     1. 读锁使用介绍：
    >     
    >        加上读锁后，也可以同时加上写锁
    >        
    >        读锁是共享锁，多个线程可以同时使用一个读锁
    >        
    >     2. 写锁
    >     
    >        写锁是独占锁，加上写锁后，不能加上读锁
    >        
    >        写锁每次只允许一个线程使用写锁，进行写操作
    >     
    >     3. 读锁和写锁，它们都可能造成死锁，造成死锁的原因：
    >     
    >        读锁的死锁：因为读锁是共享锁，因此如果有两个线程同时想要进行写操作，它们都会等待对方释放读锁然后进行写操作，导致了死锁
    >        
    >        写锁的死锁：如果对两行数据库数据，各加上写锁(因为数据库的InnoDB锁是行锁，因此可以给两行数据加上写锁)，如果有两个线程，一个给A数据加写锁，但去修改B数据；一个给B数据加写锁，但去修改A数据，则这两个线程会等待对方commit解开锁才能修改，造成死锁。
    >        
    >     4. 如何避免读锁和写锁的死锁？
    >     
    >        加上读锁，我们对于该数据只是读取，不要去修改
    >        
    >        加上写锁，我们对于该数据写操作，不去操作其他的数据
    >     
    > 3. 数据库中，间隙锁概念
    >
    >    当我们要对数据库数据进行操作时，会对数据进行上锁。如果我们SQL有范围判断，则数据库会使用间隙锁进行上锁。
    >    
    >    间隙锁会导致范围内的数据全部被锁住，因此多线程情况下严禁出现间隙锁，防止影响其他线程的插入数据操作
    >    
    > 4. ReentrantReadWriteLock(读写锁)
    >        
    >     1. 类似例子
    >     
    >        缓存，允许其他的多个线程读取，只允许一个线程来添加数据
    >        
    >     2. 介绍
    >     
    >        ReadWriteLock，是一个接口，ReentrantReadWriteLock是其实现类
    >        
    >        保证一个资源，每次只会有一个线程进行写操作，可以有多个线程进行读操作
    >        
    >     3. 使用案例：自定义一个缓存类
    >     
    >         ```
    >         class MyCache {
    >         
    >             private volatile Map<String, String> myCache = new HashMap<>();
    >         
    >             private ReadWriteLock readWriteLock = new ReentrantReadWriteLock();
    >         
    >             public void write(String key, String value) {
    >                 readWriteLock.writeLock().lock();
    >                 try {
    >                     System.out.println("开始存入数据");
    >                     myCache.put(key, value);
    >                     System.out.println("数据存入完毕");
    >                 } finally {
    >                     readWriteLock.writeLock().unlock();
    >                 }
    >             }
    >         
    >             public void read(String key) {
    >                 readWriteLock.readLock().lock();
    >                 try {
    >                     System.out.println("读取数据");
    >                     System.out.println(myCache.get(key));
    >                     System.out.println("读取数据完毕");
    >                 } finally {
    >                     readWriteLock.readLock().unlock();
    >                 }
    >             }
    >         
    >         }
    >         ```
    >         
## 13. 阻塞队列
13. 阻塞队列
    > 1. 阻塞队列，阻塞体现在什么地方？
    >    
    >    在队列中没有元素时，获取数据功能阻塞住，等待数据填入；队列全部填入，添加元素的功能阻塞，整个过程自动完成。
    >    
    > 2. 阻塞队列好处？
    >
    >    在多线程编程时，某些情况下，线程会被自动挂起(阻塞)，等到某些条件满足，被挂起的线程被自动唤醒
    >    
    >    如果自己来完成，会给程序带来很高的复杂度。但是，因为有了BlockingQueue，我们就不用关心何时挂起线程，何时唤醒线程，做到自动挂起和自动唤醒。
    >    
    > 3. 阻塞队列的具体应用例子
    >
    >    例如，我们的群发短信。群发短信，就是利用多线程，每个线程给一个用户发送短信。
    >    
    >    我们可以将用户号码放入BlockingQueue，接下来，从BLockingQueue中获取用户然后发送短信即可。如果没有号码，发送消息线程自动挂起；如果号码存满，存入号码的线程自动挂起。
    > 4. BlockingQueue
    >
    >     1. 介绍
    >     
    >        BlockingQueue，阻塞队列。BlockingQueue是一个接口，Java提供了很多的实现类
    >        
    >     2. BlockingQueue架构
    >     
    >        BlockingQueue接口，它继承了Queue接口，Queue接口继承了Collection接口。
    >        
    >     3. BlockingQueue最常用的实现类
    >     
    >         1. ArrayBlockingQueue：数组结构实现的有界阻塞队列
    >         
    >            使用：直接new，构造方法中指定容量
    >            
    >         2. LinkedBlockingQueue：链表组成的有界阻塞队列。链表组成的有界阻塞队列大小默认值为Integer.MAX_VALUE
    >         
    >            使用：直接new
    >            
    >         3. SynchronousQueue：不存储元素的阻塞队列。即单个元素阻塞队列。
    >         
    >            使用：直接new
    >         
    >     4. BlockingQueue核心方法
    >     
    >         1. 插入类别
    >         
    >             ```
    >             add(E e) -- 插入元素。阻塞队列满，会抛出异常
    >             
    >             offer(E e) -- 插入元素。插入成功，返回true；插入失败，返回false
    >             
    >             put(E e) -- 插入元素。该方法会插入元素，如果无法插入，则阻塞住生产者线程直到可以插入为止
    >             
    >             offer(E e, int time, TimeUnit unit) -- 插入元素。如果无法插入，阻塞住一段时间，之后线程退出，方法返回false。
    >             ```
    >             
    >         2. 移除类别
    >         
    >             ```
    >             remove() -- 移出元素。阻塞队列空，抛出异常。
    >             
    >             poll() -- 移出元素。移出成功，返回移出的元素；移出失败，返回null
    >             
    >             take() -- 移出元素。该方法移出元素，没有元素供移出，会阻塞住消费者线程直到可以移出
    >             
    >             poll(int time, TimeUnit unit) -- 移出元素。如果没有元素供移出，阻塞住一段时间，之后线程退出，方法返回null 。
    >             ```
    >             
    >         3. 检查类别
    >         
    >             ```
    >             element() -- 检查队列底的第一个元素。按照队列特性，先进先出，因此检查的是队列底的第一个元素。如果没有，则抛出异常。
    >             
    >             peek() -- 检查队列底的第一个元素。有则返回
    >             ```
    >     
    > 5. 下面要讲的线程池中，就使用了阻塞队列的技术
    >             
## 14. 线程池
14. 线程池
    > 1. 线程池和数据源(DataSource)的相似处
    >
    >    线程池和数据源具有相同的设计思想，它们的优点、存放位置等都是相同的
    >    
    > 2. 线程池优点：
    >
    >    1、线程复用，降低了资源的消耗
    >
    >    2、控制最大并发数(消峰)
    >
    >    3、提高了响应速度
    >
    >    3、提高线程的可管理性
    >
    > 2. 结合JVM知识，问：线程池放在JVM哪里？
    >
    >    放在养老区。因为线程池的持续时间非常长
    >    
    > 4. 线程池的架构体系：
    >
    >     Java的线程池，都是靠Executor框架来实现的。
    >
    >     Executor框架中，包含了Executor、Executors、ExecutorService、ThreadPoolExecutor这几个类
    >
    >     Executors，是线程池的工具类，就是用来创建线程池的
    >
    > 5. Executor接口的部分体系架构
    >
    >     * Executor接口
    >
    >         * ExecutorService接口：继承自Executor接口
    >         
    >             * AbstractExecutorService抽象类：实现ExecutorService接口
    >             
    >                 * ThreadPoolExecutor类：AbstractExecutorService抽象类的子类。Java线程池，最为重要的实现类，必须记住
    >                 
    >                     * ScheduledThreadPoolExecutor类：继承了ThreadPoolExecutor类，并且实现了ScheduleExecutorService接口。
    >                 
    >             * ScheduleExecutorService接口：继承自ExecutorService接口
    >
    > 6. 线程池的创建
    >
    >     1. 方式1：
    >
    >         ```
    >         Executors.newFixdThreadPool(int num) -- 创建线程池，线程池中有num个线程
    >         
    >         此种方式，适用于线程池中的线程，都是执行长期任务的线程
    >         ```
    >         
    >     2. 方式2：
    >
    >         ```
    >         Executors.newSingleThreadExecutor(); -- 创建线程池，一个线程池中只有一个线程
    >         ```
    >         
    >     3. 方式3：
    >
    >         ```
    >         Executors.newCachedThreadPool(); -- 创建线程池
    >         
    >         该方式的线程池，适用于线程池中的线程，都是执行短期任务的线程
    >         该线程池，会根据需要创建新的线程，因此可以扩容；同时，有线程池基本特性，即可以将先前构建的线程重复利用
    >         ```
    >
### 1. ThreadPoolExecutor
1. ThreadPoolExecutor
    > 1. 三种创建线程池方式的底层剖析
    >
    >     1. 三种方式的底层源码：
    >     
    >         ```
    >         public static ExecutorService newFixedThreadPool(int nThreads) {
    >         	return new ThreadPoolExcutor(nThreads, nThreads, 0L, TimeUtils.MILLSECONDS, new LinkedBlockingQueue<Runnable>());
    >         }
    >         ```
    >         
    >         ```
    >         public static ExecutorService newSingleThreadPool() {
    >         	return new ThreadPoolExcutor(1, 1, 0L, TimeUtils.MILLSECONDS, new LinkedBlockingQueue<Runnable>());
    >         }
    >         ```
    >         
    >         ```
    >         public static ExecutorService newCachedThreadPool() {
    >         	return new ThreadPoolExcutor(0, Integer.MAX_VALUE, 60L, TimeUtils.SECONDS, new SynchronousQueue<Runnable>());
    >         }
    >         ```
    >     
    > 2. ThreadPoolExecutor底层原理：ThreadPoolExecutor的构造方法，共有7个参数。这7个参数是关键，必须掌握。
    > 
    > 3. ThreadPoolExecutor的7大参数：从前往后，依次来说
    > 
    >     1. corePoolSize：线程池常驻核心线程数量
    >     
    >         * corePoolSize使用了惰性判断的机制。在new线程池时，此时没有线程；只有当我们使用execute()方法调用线程池中的线程执行任务时，才会判断当前线程池中线程数，不够corePoolSize的话增加线程
    >     
    >     2. maximumPoolSize：线程池中，能够同时容纳同时执行的最大线程数。必须大于0。
    >     
    >     3. keepAliveTime：多余的空闲线程存活时间。如果线程数量超过corePoolSize规定的数量，则多余线程会被允许存在keepAliveTime的时间，超过时间直接销毁，直到线程数剩到corePoolSize为止 
    >     
    >     4. unit：多余的空闲线程存活时间的单位。使用TimeUtil的枚举类型来指定
    >     
    >     5. workQueue：任务队列。即被提交了，但尚未被执行的任务。实参通常是阻塞队列的实例
    >     6. threadFactory：表示生成线程池中工作线程的工厂。用于创建线程，一般默认即可。
    >     
    >     7. handler：拒绝策略。表示当任务队列已经满了，并且工作线程大于等于线程池的最大容纳线程数时，如何来拒绝请求执行的Runnable的策略。
    >     
### 2. 线程池底层工作原理
2. 线程池底层工作原理
    > 1. 程序运行时架构图和线程池处理的流程图：参见视频
    > 
    >    高并发情况下，线程池执行execute()方法，会先创建核心线程，然后让核心线程处理任务；任务更多时，将多余任务放入阻塞队列，等待核心线程处理；海量任务，如果任务队列也放满了，线程池创建更多的临时线程处理任务；如果任务还是多，拒绝策略发挥作用，拒绝处理
    >    
### 3. 面试常考的考点
3. 面试常考的考点
    > 1. 创建线程池时，线程数为0。只有当执行execute()方法处理任务时，才创建线程
    >
    > 2. 调用execute()方法添加新任务时，线程池处理逻辑：
    >
    >    判断正在运行的线程数是否小于corePoolSize，是则马上创建线程执行任务，否则进入下一个判断
    >    
    >    判断任务队列是否有空闲，有则放入任务队列，没有则进入下一个判断
    >    
    >    判断当前运行线程数量是否小于maximumPoolSize，是则创建非核心线程立刻执行任务，否则执行最终
    >    
    >    最终执行拒绝策略，拒绝执行任务
    >    
    > 3. 一个线程完成任务后，会去任务队列中获取下一个任务来执行
    >
    > 4. 当线程无事可做，并且超过keepAliveTime，线程池会做判断：如果当前运行线程数大于corePoolSize，停止当前空闲线程。
    >
    > 5. 线程池完成所有任务时，此时的线程数，一定会收缩到corePoolSize大小
    > 
### 4. 面试超级大坑
4. 面试超级大坑
    > 1. 上述三种创建线程池方式，工作中使用哪种？
    > 
    >    哪种都不会使用，因为上述三种方式创建的线程池，都有问题
    >    
    >    其实根据Alibaba的Java开发手册中就可以知道，线程池的创建，必须自己使用ThreadPoolExecutor去创建，不允许使用Executors提供的方式去创建。
    >    
    > 2. Executors创建的线程池的问题
    > 
    >    FixedThreadPool和SingleThreadPool的问题：请求队列的最大长度是Integer.MAX_VALUE，可能会堆积大量的请求，导致OOM
    >    
    >    CachedThreadPool的问题：允许创建的最大线程数量为Integer.MAX_VALUE，很容易创建大量线程，导致OOM
    >    
### 5. 拒绝策略
5. 拒绝策略
    > 1. 既然线程池必须我们自己手写，因此我们必须明白拒绝策略。
    > 
    > 2. JDK内置的拒绝策略：直接通过new ThreadPoolExecutor.Xxxx()来创建拒绝策略实例
    > 
    >     1. AbortPolicy：直接抛出RejectedExecutionException异常，阻止系统正常运行。该策略是Executors创建的线程池的拒绝策略
    >     
    >     2. CallerRunsPolicy："调用者运行"，一种调节机制，该策略不会抛出异常，也不会舍弃任务，而是将某些任务返还给调用者，从而降低新任务的流量
    >     
    >     3. DiscardOldestPolicy：抛弃掉任务队列中，等待最久的任务，然后将当前任务加入队列，尝试再次提交
    >     
    >     4. DiscardPolicy：丢弃掉无法处理的任务，不抛出异常也不处理。如果允许任务丢失，这是最好的处理方式
    >     
    > 3. JDK内置的拒绝策略，它们都实现了RejectedExecutionHandler接口
    > 
### 6. 手写线程池
6. 手写线程池
    > 1. 手写线程池，手写例子
    >
    >     ```
    >     class MyThreadPool {
    >     
    >         public static ExecutorService newMyThreadPool(int coreThreads, int maxThreads, int queueCapacity) {
    >             return new ThreadPoolExecutor(coreThreads, maxThreads, 3L, TimeUnit.SECONDS,
    >                     new ArrayBlockingQueue<Runnable>(queueCapacity), Executors.defaultThreadFactory(),
    >                     new ThreadPoolExecutor.DiscardPolicy());
    >         }
    >     
    >     }
    >     ```
    >     
## 15. Java8特性复习
15. Java8特性复习
    > 1. 函数式接口再回顾
    > 
    >     1. 函数式接口就是有```@Functional```注解修饰，只有一个抽象方法的接口。
    >     
    >     2. Java提供的函数式接口，都在java.function包中
    >     
    >     3. 重要的四大函数式接口
    >     
    >         1. 消费型
    >         
    >             ```
    >             Customer<T>
    >             ```
    >             
    >         2. 供给型
    >         
    >             ```
    >             Supplier<T>
    >             ```
    >             
    >         3. 函数型
    >         
    >             ```
    >             Function<T, T>
    >             ```
    >             
    >         4. 判断型 
    >         
    >             ```
    >             Predicate<T>
    >             ```
    >     
    > 2. Stream流再回顾
    > 
    >     1. Stream流是一个数据渠道，用于对数据进行计算。
    >     
    >     2. 集合讲求数据，Stream讲求计算
    >     
    >     3. Stream流的特点
    >     
    >        1、Stream不会存储数据，它只是对数据进行处理
    >        
    >        2、Stream不会改变源对象，它处理后只会返回一个新的Stream
    >        
    >        3、Stream是延迟操作，意思是只有终结方法出现时，才会进行中间的操作，最终得到结果。
    >        
## 16. 分支合并框架
16. 分支合并框架
    > 1. 分支合并理念(Fork-Join)
    >
    >    Fork：将一个复杂事物进行拆分，拆分成一个个的原子任务
    >    
    >    Join：将拆分后的任务结果汇总，呈现出最终结果
    >    
    > 2. 分支合并框架相关的类
    >
    >     1. ForkJoinPool
    >     
    >         1. 介绍
    >         
    >            ForkJoinPool，分支合并池。
    >            
    >            ForkJoinPool和线程池有些联系，它们都是ExecutorService接口的实现类。二者可以做类比记忆
    >         
    >     2. ForkJoinTask
    >     
    >         1. 介绍
    >         
    >            ForkJoinTask，分支合并任务。
    >            
    >            ForkJoinTask和FutureTask有些联系，FutureTask实现了Future接口；ForkJoinTask实现了Future接口和Serializable接口。二者可以做类比记忆
    >     
    >     3. RecursiveTask
    >     
    >         1. 介绍
    >         
    >            RecursiveTask，递归任务
    >            
    >            RecursiveTask继承了ForkJoinTask，可以实现递归调用的任务
    >     
    > 3. 分支合并框架的使用
    >
    >     1. 创建任务类，继承RecursiveTask。泛型为计算的结果类型
    >     
    >         * 任务类中，定义base case的计算逻辑，并且定义拆分逻辑和合并逻辑
    >     
    >     2. 创建任务实例，创建分类合并池实例
    >     
    >     3. 使用分类合并池的submit(ForkJoinTask<T> task)方法，方法参数为任务实例。方法返回值为ForkJoinTask实例
    >     
    >     4. 使用ForkJoinTask实例的get()方法，该方法会阻塞直到最终任务结果被返回
    >     
    > 4. 分支合并框架使用案例：0~100个数字的累加
    >
    >     1. 0~100累加，可以看成每10个数累加，最终的值汇总
    >     
    >     2. 代码
    >     
    >         ```
    >         class MyTask extends RecursiveTask<Integer> {
    >         
    >             private static final int SCOPE = 10; // 分割范围
    >             private int begin;
    >             private int end;
    >             private int result;
    >         
    >             public MyTask(int begin, int end) {
    >                 this.begin = begin;
    >                 this.end = end;
    >             }
    >         
    >         
    >             @Override
    >             protected Integer compute() {
    >                 if (end - begin <= SCOPE) {
    >                     // 定义base case
    >                     for (int i = begin; i <= end; i++) {
    >                         result = result + i;
    >                     }
    >                 } else {
    >                     int mid = begin + (end - begin) / 2;
    >                     MyTask task = new MyTask(begin, mid);
    >                     MyTask task1 = new MyTask(mid + 1, end);
    >                     task.fork();
    >                     task1.fork();
    >         
    >                     result = task.join() + task1.join();
    >                 }
    >         
    >                 return result;
    >             }
    >         }
    >         ```
    >         
    >         ```
    >         public class ForkJoinDemo {
    >             public static void main(String[] args) throws ExecutionException, InterruptedException {
    >                 MyTask myTask = new MyTask(0, 100);
    >                 ForkJoinPool pool = new ForkJoinPool();
    >                 ForkJoinTask<Integer> submit = pool.submit(myTask);
    >                 System.out.println(submit.get());
    >             }
    >         }
    >         ```
    >         
## 17. 异步回调
17. 异步回调
    > 1. CompletableFuture
    >
    >     1. 介绍
    >     
    >        CompletableFuture也是Future接口的实现类
    >        
    >        用来执行一个同步或者异步的线程任务
    >        
    >     2. 使用
    >     
    >         * 同步任务
    >         
    >             ```
    >             // 同步
    >                     CompletableFuture<Void> future = CompletableFuture.runAsync(()->		{
    >                         System.out.println("同步任务");
    >                     }); // 创建同步线程任务
    >             
    >                     future.get(); // 阻塞获取任务结果
    >             ```
    >             
    >         * 异步任务
    >         
    >             ```
    >             // 异步
    >                     CompletableFuture<Integer> future1 = CompletableFuture.supplyAsync(()->{
    >                         System.out.println("这里是异步回调");
    >                         int a = 10/0;
    >                         return 1024; // 任务执行完毕，返回一个标识
    >                     });
    >             
    >                     future1.whenComplete((t, u)->{
    >                         // 当任务执行完毕，任务的返回值作为t传入，任务如果报错报错信息作为u传入
    >                         System.out.println(t);
    >                         System.out.println(u);
    >                     }).exceptionally(f->{
    >                         // f是报错信息
    >                         System.out.println(f.getMessage());
    >                         return 444;
    >                     });
    >             ```
    >     
    > 3. 异步回调，没有人会用它，因为我们有更好的东西，即MQ消息中间件来取代它。

