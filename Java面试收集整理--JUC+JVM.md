# Java面试收集整理(JUC+JVM)
## 1. Volatile

1. Volatile

   > 1. 介绍
   >
   >    Volatile是JVM提供的，轻量级的，同步机制
   >    
   > 2. Volatile三大特征
   >
   >     1. 保证可见性
   >     
   >     2. 禁止指令重排
   >     
   >     3. **不保证原子性**
   >     
   > 3. 为研究清楚Volatile，必须先研究JMM
   > 
### 1. JMM
1. JMM
   > 1. JMM是什么
   >    
   >     JMM，Java Memory Model，Java内存模型
   >     
   >     **JMM，是一个抽象概念，不真实存在**
   >     
   >     JMM，描述了一组规范或者规则，通过这组规则定义了程序中变量(包括实例字段、静态字段、构成数组对象的元素等等)的访问方式。
   >     
   >     我们做开发时，必须遵守JMM
   >     
   > 2. JMM对于同步机制的规定
   >    
   >      1. 线程解锁前，必须把共享内存的变量，重新刷回到主内存
   >         
   >      2. 线程加锁前，必须读取主内存的最新值，到自己的工作内存
   >         
   >      3. 加锁和解锁，必须是同一个锁
   >      
   > 3. 主内存和工作内存解释
   >    
   >     计算机中，IO效率比较：硬盘 < 内存 <  CPU(CPU不存储，因此这里是指缓存)
   >    
   >     主内存，就是计算机的真实内存，即内存条
   >    
   >     工作内存，就是每个线程独立的内存区域，每个线程都有自己的工作内存
   >     
   > 4. 主内存和工作内存工作流程
   >    
   >    JVM运行程序的实体是线程，每个线程JVM都会为它创建一个工作内存(也被叫做栈空间)，工作内存是每个线程的私有数据区域。
   >    
   >    **JMM规定，所有变量存储在主内存，主内存是共享内存区域，所有线程都可以访问**
   >    
   >    但是线程对变量进行操作(读取、赋值等)时，必须在工作内存中进行。线程操作变量，先将主内存中变量复制一份副本进入工作内存，然后再工作内存中操作副本，操作完毕，将副本刷新回主内存。每个线程之间无法访问对方的工作内存，因此线程间的通信(即值的传递)，必须通过主内存来完成。
   >    
   > 5. JMM内存模型特性之可见性
   >    
   >    因为线程操作的是工作内存中的变量副本，如果有多个线程操作一个变量，它们中有线程变量操作完成，刷新回主内存，其他的线程无法得知主内存的变量已经被修改了
   >    
   >    因此，我们必须有个机制，能做到只要主内存的变量发生变化，就通知持有旧版变量的线程，重新去获取最新的变量值。这种通知机制，就是JMM内存模型特性之一的可见性。
   >    
   > 6. JMM内存模型的特性：即多线程开发时，必须遵守的特性
   >
   >     1. 可见性
   >     
   >     2. 原子性
   >     
   >     3. 有序性
   >     
   > 7. Volatile轻量级的原因
   >
   >    只遵守了JMM要求的可见性和有序性，不遵守原子性 
   >    
   >    因此Volatile可以看成synchronized的阉割版
### 2. Volatile可见性代码验证
2. Volatile可见性代码验证
   
    > 1. 前面已经说过了，JMM可见性是什么。我们要通过代码来验证可见性
    >
    > 2. 可见性验证：
    >
    >    ```
    >    public class VolatileDemo {
    >        public static void main(String[] args) {
    >            // 1. 验证Volatile可见性
    >            // 此时，num没有添加volatile关键字修饰
    >            MyData data = new MyData();
    >            new Thread(()->{
    >                System.out.println(Thread.currentThread().getName() + "启动，开始修改");
    >                try {
    >                    TimeUnit.SECONDS.sleep(3);
    >                } catch (InterruptedException e) {
    >                    e.printStackTrace();
    >                }
    >                data.addTo60();
    >                System.out.println(Thread.currentThread().getName() + "修改完毕，num为：" + data.num);
    >            }, "AAA").start();
    >    
    >            while (data.num == 0) {
    >                // main线程等待num值变化
    >            }
    >    
    >            System.out.println(Thread.currentThread().getName() + "感知到变化"); 
    >        }
    >    }
    >    
    >    class MyData {
    >    
    >        int num = 0; // 主内存中的变量
    >    
    >        public void addTo60() {
    >            this.num = 60;
    >        }
    >        
    >    }
    >    ```
    >    
    >    * 不添加volatile关键字，AAA线程修改了data.num值并刷新到主内存中；main线程没有感知到主内存中data.num已经变化
    >    
    >    ```
    >    public class VolatileDemo {
    >        public static void main(String[] args) {
    >            // 1. 验证Volatile可见性
    >            // 此时，num添加volatile关键字修饰
    >            MyData data = new MyData();
    >            new Thread(()->{
    >                System.out.println(Thread.currentThread().getName() + "启动，开始修改");
    >                try {
    >                    TimeUnit.SECONDS.sleep(3);
    >                } catch (InterruptedException e) {
    >                    e.printStackTrace();
    >                }
    >                data.addTo60();
    >                System.out.println(Thread.currentThread().getName() + "修改完毕，num为：" + data.num);
    >            }, "AAA").start();
    >    
    >            while (data.num == 0) {
    >                // main线程等待num值变化
    >            }
    >    
    >            System.out.println(Thread.currentThread().getName() + "感知到变化");
    >        }
    >    }
    >    
    >    class MyData {
    >    
    >        volatile int num = 0; // 主内存中的变量
    >    
    >        public void addTo60() {
    >            this.num = 60;
    >        }
    >    
    >    }
    >    ```
    >    
    >    * 添加volatile关键字，AAA线程修改了data.num值并刷新到主内存中；main线程感知到了主内存中变量已经变化，作出反应。验证了volatile关键字的可见性。
    >    
    > 3. 加了volatile关键字修饰的主内存变量，只要发生变化，就会立刻通知还持有旧版变量的线程，重新获取最新版本的变量。
    > 
### 3. Volatile不保证原子性验证
3. Volatile不保证原子性验证
    > 1. 原子性指的是什么
    >
    >    和数据库事务的原子性很像，就是不可分割、完整性，即某个线程进行某个业务时，中间不可以被加塞或分割，需要整体完整。
    >    
    >    要么同时成功，要么同时失败
    >    
    > 2. Volatile不保证原子性代码验证：
    >
    >     ```
    >     public class VolatileDemo2 {
    >         public static void main(String[] args) {
    >             MyData2 data2 = new MyData2();
    >             // 开启多个线程，每个线程都执行addPlusPlus()方法多次
    >             for (int i = 0; i < 20; i++) {
    >                 new Thread(()->{
    >                     for (int j = 0; j < 1000; j++) {
    >                         data2.addPlusPlus();
    >                     }
    >                 }).start();
    >             }
    >             // 因为JVM默认会启动两个线程，一个main线程，一个GC线程。
    >             // 因此如果活跃线程数大于2，说明我们开启的线程还没有运行完毕
    >             while (Thread.activeCount() > 2) {
    >                 Thread.yield(); // 当前线程暂停
    >             }
    >             System.out.println("通过num的值，判断是否volatile是否保证原子性。num=" + data2.num);
    >         }
    >     }
    >     
    >     
    >     class MyData2 {
    >     
    >         volatile int num = 0;
    >     
    >         public void addPlusPlus() {
    >             // 通过最经典的自增运算，来判断原子性
    >             num++;
    >         }
    >     
    >     }
    >     ```
    >     
    >     * 自增操作本身不是原子操作，因此可以用自增验证加了volatile的变量是否有原子性。结论是：Volatile关键字不保证原子性。
    >     
    > 4. Volatile不保证原子性的原理介绍：还是用自增运算的例子来说明
    >    
    >    自增运算，底层有三个步骤：1、从主内存中读取变量到工作内存；2、副本变量+1；3、刷新副本变量到主内存。	
    >    
    >    如果在一个时刻，多个线程同时获取了主内存中的变量，并放入工作内存；如果多个线程都操作完毕，准备刷新回主内存，此时一个线程抢占到刷新机会，剩下的被暂时挂起；主内存变量改变，Volatile有通知机制，但是该机制没有线程切换时间快，因此被挂起线程此时获得刷新机会，会将值再次覆盖刷新，出现丢失数据情况。这就是为什么Volatile不保证原子性。
    >    
    > 5. 如何解决Volatile不保证原子性的问题？
    >    
    >    首先，我们想到加锁，使用synchronized或者直接使用Lock。但是加锁太伤性能了，如果我们只是想解决自增的原子性问题，使用锁就大材小用了
    >    
    >    我们之前学过，java.until.concurrent.atomic包下的类，都是保证了原子性的，在里面有各种AtomicInteger、AtomicLong等类，我们使用这些类作为数据类型，不就解决了原子性问题吗
    >    
    >     ```
    >    public class VolatileDemo3 {
    >        public static void main(String[] args) {
    >            MyData3 data3 = new MyData3();
    >            // 开启多个线程，每个线程都执行addPlusPlus()方法多次
    >            for (int i = 0; i < 20; i++) {
    >                new Thread(()->{
    >                    for (int j = 0; j < 1000; j++) {
    >                        data3.addPlusPlus();
    >                    }
    >                }).start();
    >            }
    >            while (Thread.activeCount() > 2) {
    >                Thread.yield(); 
    >            }
    >            System.out.println("通过num的值，判断是否是否保证原子性。num=" + data3.num);
    >        }
    >    }
    >    
    >    class MyData3 {
    >    
    >        AtomicInteger num = new AtomicInteger();
    >    
    >        public void addPlusPlus() {
    >            num.getAndIncrement();
    >        }
    >    
    >    }
    >     ```
    >    
### 4. Volatile禁止指令重排验证
4. Volatile禁止指令重排验证
    > 1. JMM有序性
    >
    >     1. 指令重排
    >     
    >        计算机执行程序时，为了提高性能，编译器和处理器会对指令进行重排序，一般过程为：源代码 》编译器优化的重排序 》指令并行的重排序 》内存系统的重排序 》最终执行的指令
    >       
    >         重排序不是乱排序，处理器在进行重排序时，必须考虑指令间的数据依赖性。比如先赋值，然后运算，这个顺序肯定无法进行重排，赋值指令和运算指令，一定有数据依赖性。
    >     
    >     2. 单线程情况下，程序最终执行结果一定和代码顺序执行的结果一致。
    >
    >     3. 多线程中线程交替执行，由于编译器优化重排序的客观存在，因此两个线程中使用的变量能否一致无法确定，结果无法预测。例如：
    >     
    >         ```
    >        class Demo {
    >         	int a, b, x, y = 0;
    >         	public void method01() {
    >         		a = 2;
    >         		x = b;
    >         	}
    >         	public void method02() {
    >         	    b = 1;
    >         	    y = a;
    >         	}
    >         }
    >         
    >         public class ResortDemo {
    >         	public static void main(String[] args) {
    >         		Demo demo = new Demo();
    >         		new Thread(()->
    >         		demo.method01(), "AA").start();
    >         		new Thread(()->
    >         		demo.method02(), "BB").start();
    >         	}
    >         }
    >        ```
    >        
    >         * 上述多线程操作，因为两个线程中，指令之间没有依赖关系，因此可以随意指令重排，变量结果无法预测。
    >        
    >             正常指令顺序：
    >             
    >                 线程AA：a = 2; x = b;
    >                 
    >                 线程BB：b = 1; y = a;
    >                 
    >                 结果：x=1,y=2
    >             
    >             重排后可能的指令顺序：
    >             
    >                 线程AA：x = b; a = 2;
    >                 
    >                 线程BB：y = a; b = 1;
    >                 
    >                 结果：x=0,y=0
    >        
    >     4. Volatile可以禁止指令重排，加了volatile关键字的变量操作时，不允许指令重排，保证了有序性。
    >     
    >     5. Volatile的原理：
    >     
    >        计算机底层，有一个CPU指令：Memeory Barrier(内存屏障)，该指令是Volatile的核心。
    >        
    >        Memory Barrier可以保证操作的顺序和某些变量的可见性，正好是Volatile的可见性和有序性
    >        
    >        Memory Barrier保证操作顺序原理：如果在指令间插入Memory Barrier，则Memory Barrier前后的指令禁止执行重排序
    >        
    >        Memory Barrier保证某些变量可见性原理：Memory Barrier会强制刷出各种CPU缓存中数据，保证CPU线程读取的是这些数据的最新版本
    >     
## 2. 单例模式在多线程环境下可能存在的安全问题
2. 单例模式在多线程环境下可能存在的安全问题
    > 1. 经典面试问题：在哪些地方，用到过volatile关键字
    >
    >     多线程中，最为常用的地方，就是单例模式中
    >     
    > 2. 普通懒汉式单例模式
    >
    >     1. 代码
    >     
    >         ```
    >         public class Demo {
    >             private static Demo instance = null;
    >             private Demo() {}
    >             public static Demo getInstance() {
    >                 if (instance == null) {
    >                     instance = new Demo();
    >                 }
    >                 return instance;
    >             }
    >         }
    >         ```
    >         
    >     2. 问题
    >     
    >        多线程情况下，必定会线程不安全
    >     
    > 3. DCL单例模式：
    >
    >     1. DCL：Double Check Lock，双端检锁机制
    >     
    >     2. 代码
    >     
    >         ```
    >         public class Demo {
    >             private static Demo instance = null;
    >             private Demo() {}
    >             public static Demo getInstance() {
    >                 if (instance == null) {
    >                     synchronized (Demo.class) {
    >                         if (instance == null) {
    >                             instance = new Demo();
    >                         }
    >                     }
    >                 }
    >                 return instance;
    >             }
    >         }
    >         ```
    >         
    >     3. 双端检锁，可以保证线程安全。但是，DCL单例模式有潜在隐患，就是指令重排的问题。
    >     
    > 4. 单例模式Volatile分析：
    >
    >     1. DCL单例模式不一定保证线程安全，因为存在指令重排的隐患
    >     DCL机制的单例模式，对象初始化的代码```instance = new Demo()```，其实计算机中共有3个步骤：1、给对象分配内存；2、初始化对象；3、设置instance指向分配的对象内存地址，instance != null
    >        
    >        但是，步骤2和步骤3不存在数据依赖问题，因此允许指令重排，重排后：1、给对象分配内存；3、设置instance指向分配内存地址，instance != null。但是此时对象并未初始化完成；2、初始化对象
    >        
    >        如果指令重排了，则可能导致instance!=null，但是对象没有初始化，此时就会造成线程安全问题
    >        
    >     2. 添加volatile关键字，禁止DCL单例模式的指令重排
    >     
    >         ```
    >         public class Demo {
    >             private static volatile Demo instance = null;
    >             private Demo() {}
    >             public static Demo getInstance() {
    >                 if (instance == null) {
    >                     synchronized (Demo.class) {
    >                         if (instance == null) {
    >                             instance = new Demo();
    >                         }
    >                     }
    >                 }
    >                 return instance;
    >             }
    >         }
    >         ```
    >         
## 3. CAS
3. CAS
    > 1. CAS是什么
    >
    >    CAS，Compare And Swap，比较并交换思想
    >    
    >    CAS是java.util.concurrent.atomic中原子类的原子性实现方式
    >    
    > 2. **java.util.concurrent.atomic包中的原子类，使用volatile和CAS，保证了JMM的多线程开发的规定，并且由于没有使用锁，保证了高并发时的效率。**
    >
    > 3. CAS在原子类中体现：以AtomicInteger为具体例子
    >
    >    AtomicInteger中，有compareAndSet(int expect, int update)方法，该方法用到了CAS。
    >
    >    compareAndSet(int expect, int update)方法，expect为线程期望此时主内存中的AtomicInteger的值为多少；update为修改的AtomicInteger的值，该方法返回值为Boolean类型
    >
    >    线程获取主内存中的AtomicInteger副本，修改后，会先调用compareAndSet(int expect, int update)方法，判断此时主内存AtomicInteger是否是期望值，是则修改主内存的值为更新值；不是，则不修改主内存的值
    >
### 1. CAS底层原理
1. CAS底层原理
    > 1. **CAS底层原理，就是靠的UnSafe类和自旋锁**
    >
    > 3. AtomicInteger类中，getAndIncrement()方法也用到了CAS，它底层实现为
    >
    >    ```
    >    public final int getAndIncrenment() {
    >    	// this就是当前的AtomicInteger对象，valueOffset是内存偏移量，1是固定写法
    >    	return unsafe.getAndAddInt(this, valueOffset, 1);
    >    }
    >    ```
    >
    >     * 这个unsafe.getAndAddInt()方法，就是重点。通过查看unsafe.getAndAddInt()了解CAS的底层实现
    >
    >     * 查看AtomicInteger源码：
    >
    >         ```
    >    private volatile int value;
    >        private static final Unsafe unsafe = Unsafe.getUnsafe();
    >        private static final long valueOffset;
    >        static {
    >    	    try {
    >    	    	valueOffset = unsafe.objectFieldOffset(AtomicInteger.class.getDeclaredField("value"));
    >    	    } catch (Exception ex) {
    >    	    	throw new Error(ex);
    >        	}
    >        }
    >        ```
    >
    >         * valueOffset是内存偏移量，通过Unsafe类根据内存偏移地址来获取数据
    >
    >         * value是volatile修饰的真实类型数据，volatile保证了多线程间的可见性和有序性
    >
    > 4. Unsafe类
    >
    >    1. 介绍
    >      
    >        Unsafe类，是JDK源码包下(jdk/jre/rt.jar中)，sun.misc包里的类，是JDK自带的类
    >        
    >        Unsafet类是CAS的核心类，由于Java无法直接访问底层系统，需要通过本地方法访问。Unsafe类相当于一个后门，使用Unsafe类可以直接访问特定内存数据
    >        
    >     2. Unsafe类的特点
    >
    >        它的所有方法，都是被native关键字修饰的。意思是它的大部分方法，都是本地方法。
    >        
    >        意思就是Unsafe类方法可以直接调用操作系统底层资源去执行相应的任务
    >
    > 5. CAS详细解释
    >
    >    **CAS，Compare-And-Swap，它是CPU的并发原语。功能是判断内存某个地址的值是否为预期值，是则变为新的值，整个过程是原子的**
    >
    >    CAS体现在Java中，就是sun.misc.Unsafe类中的各种方法。调用Unsafe类中的CAS方法，JVM帮我们实现CAS汇编指令。
    >
    >    这是通过硬件功能，实现的原子操作
    >
    >    * 原语：操作系统用语，由若干个指令组成，用于实现某个功能。原语执行必须连续，并且原语不允许被中断，也就是说CAS是CPU的原子指令，不会造成数据不一致问题。
    >
    > 6. unsafe.getAndAddInt()方法源码与解释
    >
    >    ```
    >    public final int getAndAddInt(Object var1, long var2, int var4) {
    >     	int var5;
    >     	do {
    >     		var5 = this.getIntVolatile(var1, var2);
    >     	} while(!this.compareAndSwapInt(var1, var2, var5, var5 + var4));
    >     	return var5;
    >     }
    >    ```
    >
    >     * 参数说明：
    >
    >        var1 -- AtomicInteger对象
    >        
    >        var2 -- 该对象的引用地址
    >        
    >        var4 -- 需要变动的数量
    >        
    >        var5 -- 根据var1、var2在主内存中找到的真实值
    >        
    >     * unsafe.getAndAddInt()方法执行过程：
    >
    >        根据var1和var2获取真实值，放入var5；
    >        
    >        用该对象当前值与var5比较，如果相同，更新为var5 + var4并且返回true跳出循环；如果不同，返回false，继续循环取真实值放入var5然后比较，直到更新完成
    >    
    > 6. AtomicInteger中的compareAndSet()方法源码：
    >
    >     ```
    >     public final boolean compareAndSet(int expect, int update) {
    >             return unsafe.compareAndSwapInt(this, valueOffset, expect, update);
    >     }
    >     ```
    >     
    >     * 通过源码可以看到，compareAndSet()方法底层直接使用了unsafe.compareAndSwapInt()方法，它只会比较一次。
    >     
### 2. CAS缺点
2. CAS缺点
   
    > 1. CAS有三个缺点：
    >    1. 使用do...while()循环比较，循环时间长，开销很大
    >    2. 只能保证一个共享变量的原子操作
    >    3. 有ABA问题
    >    
### 3. ABA问题
3. ABA问题
    > 1. ABA问题一句话介绍：狸猫换太子
    >
    > 2. ABA问题，形象的例子：
    >
    >    两个线程一个快一个慢，它们在某个时刻都获取了主内存中的A数据。快的将A修改为B，最后将B变成A，此时慢的反应过来了，进行CAS比较发现A还是A，以为数据没有变化过。这就是为什么说ABA是狸猫换太子
    >
    > 3. 解决ABA问题
    >
    >    如果我们可以使用版本号来标记对象引用，就可以解决ABA问题了
    >    
    >    这里就引出了带版本号的原子引用
    >
    > 4. AtomicReference原子引用
    >    
    >     1. 介绍
    >        
    >        java.util.concurrent.atomic包下有很多原子类，如果我们想让自己定义的类所产生的对象，也可以是原子的，可以使用原子引用来指向
    >        
    >        AtomicReference也是java.util.concurrent.atomic包中提供给我们的，它也有compareAndSet()、get()、set()等方法
    >     
    > 5. AtomicStampedReference版本号原子引用
    >
    >     1. 介绍
    >     
    >        可以解决ABA问题的原子引用，根据一个int类型的时间戳作为版本号
    >        
    >     2. 常用方法
    >     
    >         ```
    >         compareAndSet(V expectedReference, V newReference, int expectedStamp,
    >              int newStamp) -- 比较并交换
    >         
    >         V getReference() -- 获取版本号原子引用对应的值
    >         
    >         int getStamp() -- 获取版本号
    >         ```
    >         
    >     3. 使用AtomicStampedReference解决ABA问题实例
    >     
## 4. 集合类不安全问题
### 1. 并发修改异常
1. 并发修改异常
   > 1. ArrayList底层原理再复习
   >
   >    ArrayList底层由数组进行实现，默认初始容量为10
   >
   >    当需要扩容时，采用位运算，扩容1.5倍，变为15
   >    
   > 2. ArrayList线程不安全的体现？为什么线程不安全？
   > 
   >    ArrayLS在并发写操作时线程不安全，多线程写操作时，会抛出```java.util.ConcurrentModificationException```异常
   >    
   >    因为ArrayList为了保证效率，底层实现时方法没有使用锁
   >    
   > 3. 写一个ArrayList线程不安全的case
   > 
   >    ArrayList在并发写操作(添加、修改、删除)时，会发生线程不安全，写个并发写的demo就可以
   >    
   > 4. 如何解决ArrayList线程不安全的问题
   > 
   >     1. Vector类：它底层方法都是加了synchronized关键字的，线程安全。但是不要使用，因为太老了
   >     
   >     2. 使用Collections工具类中的synchronized前缀方法，将线程不安全的集合变成线程安全的
   >     
   >     3. 使用写时复制的集合：例如CopyOnWriteArrayList、CopyOnWriteHashSet
   >     
   >         * Map没有写时复制的Map，但是可以使用JUC提供的ConcurrentHashMap类，它是线程安全的。
   >         
### 2. 写时复制
2. 写时复制
    > 1. 引言
    >
    >    写时复制技术，最先了解到的地方是Redis中的RDB持久化
    >    
    >    JUC提供的写时复制的类，也使用到了写时复制技术
    >    
    > 2. 写时复制介绍
    >
    >     写时复制技术，其实就是读写分离思想的体现。
    >     
    >     将我们要修改的数据，复制一个副本出来，写操作在副本上进行，如果有读操作读的还是原来的数据；当写操作完毕，让副本替代原来的数据
    >     
    > 3. CopyOnWriteArrayList源码剖析
    >
    >     ```
    >     // CopyOnWriteArrayList底层，也是一个Object数组，不过被volatile修饰
    >     private tarnsient volatile Object[] arry;
    >     ```
    >
    >     ```
    >     // CopyOnWriteArrayList底层的add()方法
    >     // 它使用了Lock锁，保证在进行写操作时只有一个线程进行。写的时候，创建一个长度+1的副本数组，在副本的最后添加新元素，最后将副本替换array
    >     // 因为我们知道CopyOnWriteArrayList底层数组array被volatile修饰，volatile保证了可见性，因此修改完成后会通知其他持有该array的线程被array被修改
    >     
    >     public boolean add(E e) {
    >             final ReentrantLock lock = this.lock;
    >             lock.lock();
    >             try {
    >                 Object[] elements = getArray();
    >                 int len = elements.length;
    >                 Object[] newElements = Arrays.copyOf(elements, len + 1);
    >                 newElements[len] = e;
    >                 setArray(newElements);
    >                 return true;
    >             } finally {
    >                 lock.unlock();
    >             }
    >         }
    >     ```
    >     
### 3. Set集合
3. Set集合
    > 1. Set集合是否为线程安全的？
    > 
    >    不是线程安全的
    >    
    > 2. 如何保证Set集合线程安全：
    > 
    >     1. 使用Collections工具类提供的synchronizedSet()方法将Set集合变为线程安全的
    >     
    >     2. 使用CopyOnWriteArraySet类
    >     
    > 3. CopyOnWriteArraySet集合底层实现
    > 
    >     CopyOnWriteArraySet底层，仍然是靠CopyOnWriteArrayList实现的
    >     
    > 4. HashSet集合底层是什么？既然底层是HashMap，为什么HashSet的add()方法只放入一个值？
    > 
    >    底层就是HashMap，底层是一个初始容量为16，负载因子为0.75的HashMap
    >    
    >    HashSet的add()方法添加元素时，底层添加到了HashMap中的key上，value是HashSet提供好的一个Object常量，因此HashSet的add()方法只用添加一个元素。
    >    
    >     * 负载因子：容量*负载因子=负载容量，当超过负载容量就会触发扩容机制。负载因子就是触发扩容机制的权重。
    >    
### 4. Map
4. Map
    > 1. Map集合是否为线程安全的？
    > 
    >    不安全
    >    
    > 2. 如何保证Map集合线程安全？
    > 
    >    Map集合没有写时复制类，但是JUC提供了ConcurrentHashMap保证线程安全
    >    
## 5. Java锁
5. Java锁
   
    > 1. Java锁不一定是一种具体的对象，其实它主要是一种思想，可以通过Java代码进行思想的实现
### 1. 公平锁和非公平锁(FairLockAndNonfairLock)
1. 公平锁和非公平锁(FairLock And NonfairLock)
    > 1. 之前我们使用过可重入锁ReentranctLock，以ReentrantLock来说
    > 
    >     ```new ReentrantLock()```，直接创建ReentrantLock，相当于```new ReentrantLock(false)```。它天生就是一个非公平锁
    >     
    >     ```new ReentrantLock(true)```，创建一个公平锁。
    >     
    > 2. 公平锁和非公平锁介绍
    > 
    >    公平锁：线程按照申请锁的顺序，一个一个来获取锁
    >    
    >    非公平锁：线程获取锁的顺序，不是严格按照申请锁的顺序的。可能后申请锁的线程，比先申请锁的线程更快获取到锁。**在高并发情况下，非公平锁可能会造成优先级反转或饥饿现象**
    > 3. 公平锁和非公平锁区别：
    > 
    >    公平锁：线程先查看此锁维护的等待队列，如果为空或者该锁就是队列第一个，直接占用锁；不是，就进入等待队列
    >    
    >    非公平锁：线程先去尝试争抢锁，抢到了就占用锁；没抢到，就采用公平锁的策略
    >    
    > 4. 非公平锁优点在于吞吐量比公平锁大。ReentrantLock默认为非公平锁，synchronized而言也是非公平锁
    > 
### 2. 可重入锁
2. 可重入锁
    > 1. **可重入锁，又叫递归锁**
    > 
    > 2. 可重入锁是什么？
    > 
    >    同一个线程，它调用的外层方法获取到了锁，进入内层方法自动获取锁。即同一个线程可以访问任何一个它持有的锁同步的代码块。
    >    
    > 3. 可重入锁：ReentrantLock和synchronized是典型的可重入锁
    > 
    > 4. 可重入锁的好处
    > 
    >    **可重入锁可以避免死锁**
    >    
    > 4. 只要有上锁和解锁配对，不管对一个锁对象，嵌套执行多少次上锁和解锁，都是正确的。
    > 
### 3. 自旋锁
3. 自旋锁
    > 1. 自旋锁介绍
    >
    >    自旋锁，是指尝试获取锁的线程不会立刻阻塞，而是采用循环的方式去尝试获取锁。
    >    
    > 2. 自旋锁好处与缺点
    >
    >    自旋锁好处是减少了线程上下文切换的开销，缺点是循环会消耗CPU
    >    
    > 3. CAS就使用到了自旋锁：
    >    
    >    * unsafe.getAndAddInt()方法，底层就是使用自旋锁实现：
    >    
    >      ```
    >      public final int getAndAddInt(Object var1, long var2, int var4) {
    >       	int var5;
    >       	// 使用自旋锁
    >       	do {
    >       		var5 = this.getIntVolatile(var1, var2);
    >       	} while(!this.compareAndSwapInt(var1, var2, var5, var5 + var4));
    >       	return var5;
    >       }
    >      ```
    >    
    > 4. 手写一个自旋锁
    >    
    >    自旋锁实现，其实就是CAS+循环
    >    
    >    ```
    >    /**
    >     * 自旋锁的demo
    >     */
    >    class SpinLockDemo {
    >    
    >        // 使用原子引用线程
    >        AtomicReference<Thread> reference = new AtomicReference<>();
    >    
    >        /**
    >         * 使用自旋锁，对线程上锁
    >         */
    >        public void myLock() {
    >            // 1. 获取当前线程
    >            Thread thread = Thread.currentThread();
    >            // 2. 使用自旋锁，对线程上锁
    >            /*
    >                原子引用线程，初始值肯定为null。
    >                线程进入，比较原子引用线程初始值，如果为null，则将当前线程放入，相当于上锁
    >                之后其他线程想要上锁，因为原子引用线程值不为null，则会循环等待
    >             */
    >            while (!reference.compareAndSet(null, thread)) {}
    >        }
    >    
    >        /**
    >         * 解锁
    >         */
    >        public void myUnlock() {
    >            // 1. 获取当前线程
    >            Thread thread = Thread.currentThread();
    >            // 2. 将当前线程从原子引用线程中去除
    >            reference.compareAndSet(thread, null);
    >        }
    >        
    >    }
    >    ```
    >    
### 4. 独占锁、共享锁、互斥锁
4. 独占锁、共享锁、互斥锁
    > 1. 独占锁和共享锁
    >
    >    独占锁就是写锁，共享锁就是读锁
    >
    >    独占锁，每次只能被一个线程所持有。ReentrantLock和synchronized都是独占锁
    >
    >    共享锁，可以被多个线程持有
    >    
    > 2. 读操作和写操作、写操作和写操作之间是互斥的
    >
    > 3. 有的时候，我们必须满足在高并发情况下，多个线程可以同时对资源进行共享读和独占写，这里引出了我们的读写锁ReentrantReadWriteLock
    >
    > 4. ReadWriteLock接口
    >
    >     1. 介绍
    >        
    >        JUC提供的，能够同时满足共享读和独占写的读写锁接口
    >        
    >        ReentrantReadWriteLock类，是ReadWriteLock的实现类
    >        
    >     2. 代码验证：
    >     
    >         * 首先，手写一个资源类
    >         
    >            该资源类是一个高并发环境下的缓存资源类，因此：底层数据必须被volatile修饰、有读方法、有写方法、有清空方法
    >            
    >             ```
    >            /**
    >             * 资源类
    >             */
    >            class MyCache {
    >            
    >                private volatile Map<String, Object> dataMap = new HashMap<>();
    >                private ReadWriteLock readWriteLock = new ReentrantReadWriteLock();
    >                /**
    >                 * 读操作
    >                 * 读操作满足共享
    >                 * @param key
    >                 * @return
    >                 */
    >                public Object get(String key) {
    >            
    >                    readWriteLock.readLock().lock();
    >            
    >                    try {
    >                        System.out.println(Thread.currentThread().getName() + "\t 正在读取");
    >                        TimeUnit.MILLISECONDS.sleep(300);
    >                        System.out.println(Thread.currentThread().getName() + "\t 读取完毕");
    >            
    >                        return dataMap.get(key);
    >                    } catch (InterruptedException e) {
    >                        e.printStackTrace();
    >                        return null;
    >                    } finally {
    >                        readWriteLock.readLock().unlock();
    >                    }
    >                }
    >            
    >                /**
    >                 * 写操作
    >                 * 写操作必须满足原子+独占
    >                 * @param key
    >                 * @param value
    >                 */
    >                public void put(String key, Object value) {
    >            
    >                    readWriteLock.writeLock().lock();
    >                    try {
    >                        System.out.println(Thread.currentThread().getName() + "\t 正在写入");
    >                        try {
    >                            TimeUnit.MILLISECONDS.sleep(300);
    >                        } catch (InterruptedException e) {
    >                            e.printStackTrace();
    >                        }
    >                        dataMap.put(key, value);
    >                        System.out.println(Thread.currentThread().getName() + "\t 写入完成");
    >                    } catch (Exception e) {
    >                        e.printStackTrace();
    >                    } finally {
    >                        readWriteLock.writeLock().unlock();
    >                    }
    >            
    >                }
    >            
    >                public void empty() {
    >                    dataMap.clear();
    >                }
    >                
    >            }
    >             ```
    >            
## 6. JUC工具类
### 1. CountDownLatch
1. CountDownLatch
   
    > 1. 介绍
    >
    >    减数计数器
    >
    >    
#### 1. 小知识点：枚举类
1. 小知识点：枚举类
    > 1. 枚举类相当于一个小的数据库
    > 
    > 2. 使用
    > 
### 2. CyclicBarrier
2. CyclicBarrier
    > 1. 介绍
    >    循环栅栏
    >    
### 3. Semaphore
3. Semaphore
    > 1. 介绍
    >    信号灯，也叫信号量
    >    
## 7. 阻塞队列
7. 阻塞队列
    > 1. 阻塞队列介绍
    >
    >    阻塞队列为空，获取阻塞队列元素的线程被阻塞
    >    
    >    阻塞队列满了，往阻塞队列中放入元素的线程被阻塞
    >    
    > 2. 阻塞队列的架构
    >
    >    BlockingQueue接口，它继承了Queue接口，Queue接口继承了Collection接口。
    >    
    > 3. BlockingQueue最常用的实现类
    >
    >     * ArrayBlockingQueue
    >     
    >     * LinkedBlockingQueue
    >     
    >     * SynchronousQueue
    >     
    > 4. 阻塞队列常用方法
    >
    >     1. 抛出异常的方法
    >     
    >         ```
    >         add(E e) -- 向阻塞队列添加元素
    >         
    >         remove() -- 移除队列底的元素
    >         remove(Object o) -- 定点移除队列中的元素
    >         
    >         element() -- 检查队列是否为空，不为空则返回队列队首元素
    >         ```
    >     
    >     2. 返回布尔值的方法
    >     
    >         ```
    >         offer(E e) -- 向阻塞队列添加元素
    >         
    >         poll() -- 移除队列底的元素
    >         
    >         peek() -- 检查队列是否为空，不为空则返回队列队首元素
    >         ```
    >     
    >     3. 会让线程阻塞的方法
    >     
    >         ```
    >         put(E e)
    >         
    >         take()
    >         ```
    >         
    >     4. 会让线程阻塞一段时间，之后线程不再阻塞的方法
    >     
    >         ```
    >         offer(e, time, unit)
    >     
    >         poll(time, unit)
    >         ```
    >         
### 1. SynchronousQueue同步队列
1. SynchronousQueue同步队列
    > 1. SynchronousQueue是比较难懂的，因此单独讲
    > 
    > 2. SynchronousQueue介绍
    > 
    >    SynchronousQueue没有容量
    >    
    >    它与其他的BlockingQueue实现类不同，它是不存储元素的BlockingQueue
    >    
    >    对于SynchronousQueue，每一个put操作，必须要等待一个take操作，否则不能继续添加元素；反之也是如此。
    >    
### 2. 生产者消费者问题--传统版
2. 生产者消费者问题--传统版
    > 1. 代码实例
    >
    >     ```
    >     class TraditionalData {
    >         private int number = 0;
    >         private Lock lock = new ReentrantLock();
    >         private Condition condition = lock.newCondition();
    >     
    >         public void incr() {
    >             lock.lock();
    >             try {
    >                 // 判断
    >                 while (number != 0) {
    >                     try {
    >                         condition.await();
    >                     } catch (InterruptedException e) {
    >                         e.printStackTrace();
    >                     }
    >                 }
    >     
    >                 // 生产
    >                 number++;
    >                 System.out.println(Thread.currentThread().getName() + "\t number=" + number);
    >     
    >                 // 通知
    >                 condition.signal();
    >             } finally {
    >                 lock.unlock();
    >             }
    >         }
    >     
    >         public void dec() {
    >             lock.lock();
    >             try {
    >                 // 判断
    >                 while (number == 0) {
    >                     try {
    >                         condition.await();
    >                     } catch (InterruptedException e) {
    >                         e.printStackTrace();
    >                     }
    >                 }
    >     
    >                 // 消费
    >                 number--;
    >                 System.out.println(Thread.currentThread().getName() + "\t number=" + number);
    >     
    >                 // 通知
    >                 condition.signal();
    >             } finally {
    >                 lock.unlock();
    >             }
    >         }
    >     }
    >     ```
    >     
### 3. synchronized和Lock有什么区别？使用Lock有什么好处？
3. synchronized和Lock有什么区别？使用Lock有什么好处？

    > 1. synchronized是Java关键字，Lock是Java提供的类；synchrozied使用时，不需要手动释放锁，当synchronized代码执行后系统会自动释放锁，Lock使用时需要手动释放锁，否则会造成死锁；synchronized等待不可以中断，除非抛出异常或者正常运行完成，Lock可以中断，比如tyrLock(Long timeout, TimeUnit unit)设置超时；synchronized一定是非公平锁，Lock默认是非公平锁但是也可以设置成公平锁；synchronized不可以为锁绑定多个条件，Lock可以绑定多个条件达到精确唤醒。
    > 
    > 2. Lock好处是可以中断，并且可以设置多个条件达到精确唤醒的目的
    > 
### 4. 生产者消费者问题--阻塞队列版
4. 生产者消费者问题--阻塞队列版
   
    > 1. 通过同步阻塞队列，让程序员不用自己去进行wait和signal，交给阻塞队列。当开关打开，生产者和消费者自动进行交互作业；开关关闭，关闭作业。
    >
    > 2. 代码
    >
    >    ```
    >    class ModernData {
    >        private volatile boolean FLAG = true; // 开关，默认开启
    >        private AtomicInteger atomicInteger = new AtomicInteger();
    >        BlockingQueue<String> blockingQueue = null;
    >    
    >        public ModernData(BlockingQueue<String> queue) {
    >            this.blockingQueue = queue;
    >            System.out.println(blockingQueue.getClass().getName()); // 日志排查
    >        }
    >    
    >        /**
    >         * 生产
    >         * @throws Exception
    >         */
    >        public void myProduce() throws Exception{
    >            String data = null;
    >            boolean offer;
    >            while (FLAG) {
    >                data = atomicInteger.incrementAndGet() + "";
    >                offer = blockingQueue.offer(data, 4L, TimeUnit.SECONDS);
    >                if (offer) { // 说明当前生产者生产的数据，放入了阻塞队列
    >                    System.out.println(Thread.currentThread().getName() + "\t 放入数据为:" + data);
    >                } else {
    >                    System.out.println(Thread.currentThread().getName() + "\t 放入数据失败");
    >                }
    >                TimeUnit.SECONDS.sleep(2); // 假设每次生产耗时2秒
    >            }
    >            System.out.println("停止生产，FLAG变为false");
    >        }
    >    
    >        /**
    >         * 消费
    >         * @throws Exception
    >         */
    >        public void myCustomer() throws Exception {
    >            String data = null;
    >            while (FLAG) {
    >                data = blockingQueue.poll(4L, TimeUnit.SECONDS);
    >                if (null == data || data.equalsIgnoreCase("")) { // 生产者停止生产
    >                    FLAG = false;
    >                    System.out.println(Thread.currentThread().getName() + "\t 超过2秒没有取到数据，消费停止");
    >                    return;
    >                }
    >                System.out.println(Thread.currentThread().getName() + "\t 消费者获取到数据：" + data);
    >            }
    >        }
    >    
    >        /**
    >         * 停止
    >         */
    >        public void stop() {
    >            this.FLAG = false;
    >        }
    >    }
    >    ```
    >    
## 8. Callable接口
8. Callable接口
    > 1. 介绍
    >
    >    是实现多线程的第3种方式
    >    
    > 2. 与Runnable接口的异同
    >
    >    Callable接口和Runnable接口都是函数式接口
    >    
    >    Callable接口有返回值，并且可以抛出异常
    >    
    >    Callable接口要实现call()方法，Runnable接口实现run()方法
    >    
    > 3. 使用
    >
    >    Thread类构造方法中，没有传入Callable接口的重载，只有传入Runnable接口的构造方法
    >    
    >    如何将Thread和Callable联系起来？Java使用适配器模式帮助我们完成了这件事，这就引出了FutureTask类。
    >    
## 9. 线程池
9. 线程池
    > 1. 线程池优势
    >
    >    1. 线程复用，降低了资源的消耗
    >    
    >    2. 可以控制并发数量
    >    
    >    3. 提高了响应的速度
    >    
    >    4. 提高了线程的可管理性
    >    
    > 2. Java提供创建线程池的方式：
    >
    >    使用Executors工具类创建线程池，一共有5种，但是重要的有3种
    >    
    >     ```
    >    ExecutorService Executors.newFixedThreadPool(int num) -- 创建固定线程数线程池。适用于线程池中线程执行长期任务。
    >    
    >    ExecutorService Executors.newSingleThreadExecutor()
    >    
    >    ExecutorService Executors.newCachedThreadPool() -- 创建可扩容的线程池。适用于线程池中线程执行短期任务。
    >     ```
    >    
    > 3. 使用Java提供的线程池
    >
    >    使用线程池，一定记住使用try...catch...finally，finally中放线程池关闭方法。
    >    
    >     ```
    >    public static void main(String[] args) {
    >            ExecutorService threadPool = Executors.newFixedThreadPool(5);
    >            try {
    >                for (int i = 0; i < 10; i++) {
    >                    threadPool.submit(() -> {
    >                        System.out.println(Thread.currentThread().getName() + "\t 办理业务");
    >                        try {
    >                            TimeUnit.SECONDS.sleep(2);
    >                        } catch (InterruptedException e) {
    >                            e.printStackTrace();
    >                        }
    >                    });
    >                }
    >            } catch(Exception e) {
    >                e.printStackTrace();
    >            } finally {
    >                threadPool.shutdown();
    >            }
    >        }	
    >     ```
    >    
### 1. 线程池底层原理
1. 线程池底层原理
    > 1. 线程池底层都是使用的ThreadPoolExecutor实例。
    >
    > 2. ThreadPoolExecutor构造器的7大参数
    >
    >     1. corePoolSize：线程池常驻核心线程数量
    >        * corePoolSize使用了惰性判断的机制。在new线程池时，此时没有线程；只有当我们使用execute()方法调用线程池中的线程执行任务时，才会判断当前线程池中线程数，不够corePoolSize的话增加线程
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
### 2. 使用哪个线程池
2. 使用哪个线程池
    > 1. 一个也不用，手写线程池
    > 
### 3. 线程池合理配置线程数
3. 线程池合理配置线程数
    > 1. 如何配置线程池线程数？你是如何考虑的？
    > 
    >    根据业务不同，有两种考虑方式：CPU密集型和IO密集型
    >    
    >    CPU密集型：如果项目需要大量的运算，就是CPU密集型。CPU密集型线程数=CPU核数+1
    >    
    >    IO密集型：如果项目需要大量的IO操作，就是IO密集型。IO密集型线程数=CPU核数*2 或者 IO密集型线程数=CPU核数/(1-阻塞系数)，阻塞系数一般配置成0.8~0.9之间
    >    
    >    * 通过Java代码获取当前服务器CPU数量：```Runtime.getRuntime().availableProcessors()```，该代码返回CPU核心数
    >    
## 10. 死锁编码及定位分析
10. 死锁编码及定位分析
    > 1. 产生死锁的原因
    >
    >    两个及两个以上的进程执行过程中，因为争夺资源造成的等待现象。
    >    
    > 2. 写一个死锁
    >
    >     ```
    >     public class DeathLockDemo {
    >         public static void main(String[] args) {
    >             String lockA = "lockA";
    >             String lockB = "lockB";
    >     
    >             new Thread(new DeathLockData(lockA, lockB)).start();
    >             new Thread(new DeathLockData(lockB, lockA)).start();
    >         }
    >     }
    >     
    >     class DeathLockData implements Runnable {
    >     
    >         private String lockA;
    >         private String lockB;
    >     
    >         public DeathLockData(String lockA, String lockB) {
    >             this.lockA = lockA;
    >             this.lockB = lockB;
    >         }
    >     
    >         @Override
    >         public void run() {
    >             synchronized (lockA) {
    >                 System.out.println(Thread.currentThread().getName() + "\t 自己持有锁" + lockA + " \t 尝试获取锁" + lockB);
    >                 synchronized (lockB) {
    >                     System.out.println(Thread.currentThread().getName() + "\t 获取到了" + lockB);
    >                 }
    >             }
    >         }
    >     }
    >     ```
    >     
    > 3. 证明死锁：根据Java故障排查，证明死锁
    > 
    >     1. 先查看Java运行程序进程
    >     
    >        windows下，有类似Linux下```ps -ef|grep xxx```的查看Java运行程序进程的命令。
    >        
    >        jdk的bin目录下，有一个jps.exe，就是用来查看Java运行程序进程的命令文件。因为我们配置了环境变量，因此在Windows下任何目录都可以使用该jps命令
    >        
    >        在IDEA中，使用快捷键alt+F12就可以进入当前项目所在的cmd窗口，输入```jps -l```获取当前项目正在运行的所有进程的进程信息
    >        
    >     2. 根据异常所在进程编号，打印出异常的信息
    >     
    >        输入```jstack 异常进程进程号```，就会打出异常信息
    >        
    >     3. 根据打印出的异常信息，证明是死锁
    >    
    > 4. 解决死锁问题
    > 
    >    根据前面排查出的死死锁所在的代码位置，修改逻辑，解决死锁
    >    
## 11. JVM的GC快速回顾复习
11. JVM的GC快速回顾复习
    
    > 1. JVM体系结构
    > 2. GC算法分类
    > 
## 12. GC垃圾判断和GCRoot
12. 如何判断垃圾，谈谈对GCRoots的理解
    > 1. 如何判断垃圾
    > 
    >    内存中不再使用到的空间，就是垃圾
    >    
    > 2. 要进行垃圾回收，如何判断一个对象可以被回收？
    > 
    >     1. 引用计数法
    >     
    >         1. 介绍
    >         
    >            Java中要操作对象，都是操作的对象引用。因此，给对象添加一个引用计数器，引用到它时计数器+1，引用失效计数器-1，计数器为0时，该对象就可回收
    >            
    >         2. 主流的JVM都不采用该方法，因为无法解决循环引用的问题
    >         
    >     2. 枚举根节点做可达性分析(根搜索路径)
    >     
    >         1. 介绍
    >         
    >            为了解决引用计数法的循环引用问题，Java使用了可达性分析。
    >            
    >            **GCRoot就是一组必须活跃的引用。一组GCRoot，放在一个GC Root Set中**
    >            
    >            基本思想就是，通过一系列的GCRoot作为根节点，向下搜索，如果一个对象到GCRoot没有任何引用链相连，就说该对象不可用，是垃圾。
    >            
    >         2. 哪些对象可以作为GCRoot
    >         
    >             1. 虚拟机栈(栈中的局部变量区，也叫局部变量表表)中引用的对象
    >             
    >             2. 方法区中，类静态变量引用的对象
    >             
    >             3. 方法区中，常量引用的对象
    >             
    >             4. 本地方法栈中，native方法引用的对象
    >             
## 13. JVM标配参数和X参数
13. JVM标配参数和X参数
    > 1. JVM的参数类型
    >
    >     1. 标准参数：
    >     
    >         ```
    >         -version
    >         
    >         -help
    >         
    >         java -showversion
    >         ```
    >     
    >     2. X参数(了解)
    >     
    >         ```
    >         -Xint -- 解释执行
    >         
    >         -Xcomp -- 第一次使用就编译成本地代码
    >         
    >         -Xmixed -- 混合模式
    >         ```
    >     
    >     3. XX参数(重中之重)
    >     
    >         1. boolean类型
    >         
    >             ```
    >             -XX:+属性 
    >             -XX:-属性
    >             +表示开启，-表示关闭
    >             ```
    >             
    >         2. kv设置类型
    >         
    >             ```
    >             -XX:属性键=属性值
    >             ```
    > 2. 如何查看一个正在运行状态的Java程序，它的JVM参数？
    > 
    >     1. 先使用JDK的jps命令，查看当前程序的进程号：```jps -l```
    >     
    >     2. 使用jinfo命令，查看当前程序的各种信息：```jinfo -flag 想要查看的信息 进程号```，例如查看打印GC信息的功能是否开启，写成```jinfo -flag PrintGCDetails 进程号```
    >     
    > 3. JVM初始状态下，元空间、堆内存的默认值
    > 
    >    元空间默认值大概21MB
    >    
    > 4. **JVM的XX参数大坑之-Xms参数和-Xmx参数**
    > 
    >    -Xms和-Xmx参数是什么呢？
    >    
    >    这两个参数其实是简写，它们本来是XX参数类型的k-v设值类型。
    >    
    >    -Xms等价于-XX:InitialHeapSize
    >    
    >    -Xmx等价于-XX:MaxHeapSize
    >    
## 14. JVM查看参数默认值
14. JVM查看参数默认值
    > 1. 不太正规的做法
    > 
    >    上述中讲了，可以使用jps和jinfo查看信息，我们在不修改参数情况下，看到的参数信息就是默认信息   
    >    
    > 2. 正规做法(必须记住)：
    > 
    >     1. ```java -XX:+PrintFlagsInitial -version```
    >     
    >        使用该参数，查看所有的默认值
    >        
    >     2. ```java -XX:+PrintFlagsFinal -version```
    >     
    >        使用改参数，产看修改过值的参数
    >        
    >     3. ```java -XX:+PrintCommandLineFlags -version```
    >     
    >        使用该参数，打印出常用参数的信息
    >        
    >        该参数最为重要，因为最后一个参数，就是此时JVM使用的垃圾回收器是什么
    >     
    > 3. 分析打印的参数信息
    > 
    >    如果打印出来的参数，使用=，说明使用的默认值
    >        
    >    如果打印出来的参数，使用:=，说明修改过值
    >    
## 15. JVM常用基础参数盘点
15. JVM常用基础参数盘点
    > 1. 常用基础参数
    >
    >     * ```-Xss```，设置单个线程栈大小，一般默认512k~1024k之间。**64bit的Linux下，单个线程栈大小为1024k**
    >     
    >         1. ```-Xss```等价于```-XX:ThreadStackSize```
    >     
    >         2. **如果查看```-Xss```的初始值，会发现初始值为0。为0表示栈大小使用出厂的默认大小**
    >         
    >     * ```-Xms```，初始内存大小，默认为物理内存 / 64
    >     
    >     * ```-Xmx```，最大分配内存大小，默认为物理内存 / 4
    >     
    >     * ```-XX:MetaspaceSize=空间大小```，设置元空间大小。元空间不再虚拟机中，在本地内存，因此默认情况下大小受本地内存限制
    >     
    >     * ```-XX:+PrintGCDetails```，开启打印垃圾回收详细信息。非常重要，如果JVM调优必须要用它打印GC信息
    >     
    >         1. 垃圾回收详细信息详解：我们设置比较小的堆内存，然后故意new一个大对象，然后学习垃圾回收时的信息
    >         
    >             ```
    >             [GC (Allocation Failure) [PSYoungGen: 1851K->488K(2560K)] 1851K->708K(9728K), 0.0011091 secs] [Times: user=0.00 sys=0.00, real=0.00 secs] 
    >             [GC (Allocation Failure) [PSYoungGen: 488K->504K(2560K)] 708K->796K(9728K), 0.0008066 secs] [Times: user=0.00 sys=0.00, real=0.00 secs] 
    >             [Full GC (Allocation Failure) [PSYoungGen: 504K->0K(2560K)] [ParOldGen: 292K->620K(7168K)] 796K->620K(9728K), [Metaspace: 3420K->3420K(1056768K)], 0.0061952 secs] [Times: user=0.14 sys=0.02, real=0.01 secs] 
    >             [GC (Allocation Failure) [PSYoungGen: 0K->0K(2560K)] 620K->620K(9728K), 0.0007593 secs] [Times: user=0.00 sys=0.00, real=0.00 secs] 
    >             [Full GC (Allocation Failure) [PSYoungGen: 0K->0K(2560K)] [ParOldGen: 620K->603K(7168K)] 620K->603K(9728K), [Metaspace: 3420K->3420K(1056768K)], 0.0062514 secs] [Times: user=0.00 sys=0.00, real=0.01 secs] 
    >             ```
    >             
    >             * **可见，GC信息模板为：**
    >             
    >                 ```
    >                 [GC类型 [PSYoungGen:YoungGC前新生代大小->YoungGC后新生代大小(新生代总共大小)]YoungGC前JVM堆内存->YoungGC后JVM堆内存(JVM堆内存总大小),YoungGC耗时][Times: 用户耗时 系统耗时 YoungGC真实耗时]]
    >                 ```
    >                 
    >                 ```
    >                 [GC类型 [PSYoungGen:YoungGC前新生代大小->YoungGC后新生代大小(新生代总共大小)][PSOldGen:YoungGC前老年代大小->YoungGC后老年代大小(老年代总共大小)]YoungGC前JVM堆内存->YoungGC后JVM堆内存(JVM堆内存总大小),YoungGC耗时][Times: 用户耗时 系统耗时 YoungGC真实耗时]]
    >                 ```
    >         
    >     * ```-XX:SurvivorRatio```，调整伊甸区占新生代比例。默认伊甸区:幸存者0区:幸存者1区=8:1:1，一般不需要自己调整，使用默认即可
    >     
    >     * ```-XX:NewRatio```，调整老年代占JVM堆的比例。默认老年代:新生代=2:1。
    >     
    >     * ```-XX:MaxTenuringThreshold```，调整垃圾的最大年龄，即多少次普通GC后从新生区才能到养老区。默认设置是15。如果设置为0，表示年轻代的对象创建后，不放入幸存区，直接进入老年代。**垃圾最大年龄只能设置0~15**
    >     
## 16. Java四种引用
16. Java四种引用
    > 1. 引用，Reference，在Java中也是一个类
    >
    > 2. 整体架构
    >
    >     * Object
    >     
    >         * Reference
    >         
    >             * SoftReference
    >             
    >             * WeakReference
    >             
    >             * PhantomReference
    >             
    >         * ReferenceQueue
    >     
    > 2. Java四种引用介绍
    >
    >     1. 强引用
    >     
    >         1. 介绍
    >     
    >            默认支持强引用，例如```Thread a = new Thread();```，a就是强引用。
    >            
    >            强引用是最最常用的引用，几乎95%的引用都是强引用 
    >            
    >         2. 强引用特点：垃圾回收不会回收强引用的对象，即使对象不会使用，即使OOM，死也不收。
    >     
    >         3. 实例代码
    >     
    >             ```
    >             public static void main(String[] args) {
    >                     Object o1 = new Object();
    >                     Object o2 = o1;
    >                     Object o3 = o2;
    >                     o2 = null; // 强制让引用链断裂
    >                     o1 = null; // 强制让引用链断裂
    >                     System.gc(); // 开启垃圾回收
    >                     System.out.println(o3); // 因为o3是强引用，因此垃圾对象没有被回收
    >                 }
    >             ```
    >     
    >     2. 软引用
    >     
    >         1. 介绍
    >     
    >            比强引用稍弱的引用
    >     
    >         2. 软引用特点：内存足够，GC不回收软引用对象；如果即将OOM，软引用垃圾对象会被回收
    >         3. 软引用通常用在对内存非常敏感的程序中，比如做高速缓存时会用到软引用。
    >     
    >         4. 实例代码
    >     
    >             ```
    >             public static void main(String[] args) {
    >                     Object o1 = new Object();
    >                     SoftReference<Object> o2 = new SoftReference<>(o1); // 对o1的对象进行软引用
    >                     System.out.println(o1);
    >                     System.out.println(o2.get());
    >             
    >                     o1 = null; // 强制让引用链断裂
    >                     System.gc(); // 进行垃圾回收
    >                     System.out.println(o1);
    >                     System.out.println(o2.get()); // 如果内存足够，不会回收
    >                 }
    >             ```
    >     
    >     3. 弱引用
    >     
    >         1. 介绍
    >     
    >            生命周期比软引用更短
    >            
    >         2. 无论内存是否够用，只要进行垃圾回收了，弱引用垃圾对象被回收
    >     
    >         3. 实例代码
    >     
    >             ```
    >             public static void main(String[] args) {
    >                     Object o1 = new Object();
    >                     WeakReference<Object> o2 = new WeakReference<>(o1); // 弱引用
    >                     o1 = null; // 强制让引用链断裂
    >                     System.gc();
    >                     System.out.println(o1);
    >                     System.out.println(o2.get()); // 弱引用的垃圾对象被回收
    >                 }
    >             ```
    >     
    >     4. 虚引用
    >     
    >         1. 介绍
    >         
    >            虚引用就是形同虚设的引用
    >            
    >            和上述引用不同，不会对对象生命周期产生任何影响
    >            
    >         2. 一个对象如果只有虚引用在引用它，它就和没有引用一样，随时会被垃圾回收器回收。虚引用的get()方法永远为null，因此无法访问虚引用引用的对象
    >         
    >         3. 虚引用无法单独使用，只能和引用队列ReferenceQueue一起使用。
    > 
### 1. 软引用和弱引用的应用场景
1. 软引用和弱引用的应用场景
    > 1. 应用场景
    >    
    >    假如有一个应用需要读取大量的本地图片，如果每次读取都从硬盘中读取，效率很低；如果一次性全部加入内存，可能造成溢出。
    >        
    >    我们可以用软引用或弱引用解决该问题：将图片路径和图片对象的关联用一个HashMap保存，内存不足时，JVM自动回收这些缓存图片占用的内存：
    >        
    >    ```HashMap<String, SoftReference<Bitmap>> imageCache = new HashMap<String, SOftReference<Bitmap>>(); ```
    >    
### 2. WeakHashMap
2. WeakHashMap
    > 5. 知道弱引用，谈谈WeakHashMap
    >
    >     1. 介绍
    >     
    >        是java.util包下的一个类，实现了Map接口
    >        
    >        它底层使用了弱引用，会导致如果键作为垃圾对象，使用了垃圾回收后，该key-value会被垃圾回收器清除
    >        
    >     2. 可以用来做key-value的高速缓存
    >     
    >     3. 实例代码
    >     
    >         ```
    >         public static void main(String[] args) {
    >                 WeakHashMap<Integer, String> weakHashMap = new WeakHashMap<>();
    >                 Integer key = new Integer(1);
    >                 String value = "WeakHashMap";
    >                 weakHashMap.put(key, value);
    >         
    >                 System.out.println(weakHashMap);
    >                 key = null; // 手动将key置为空，引用对象成为垃圾
    >                 System.out.println(weakHashMap);
    >         
    >                 System.gc(); // 进行垃圾回收
    >                 System.out.println(weakHashMap); // key-value被垃圾回收器清理
    >             }
    >         ```
    >         
### 3. ReferenceQueue
3. ReferenceQueue
    
    > 1. 


