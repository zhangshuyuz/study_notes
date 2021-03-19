# netty
## 1. 推荐netty学习用书

1. 推荐netty学习用书

   > 1. netty in action
   > 
## 2. netty是什么
2. netty是什么
   > 1. netty是一个异步高性能网络通信框架，经常作为基础通信组件被RPC框架使用
   > 
   > 2. netty基于NIO模型
   > 
## 3. Java的BIO编程
3. Java的BIO编程
   > 1. BIO介绍
   > 
   >    BIO是阻塞IO，即Java原生IO
   >    
   > 2. I/O模型基本说明
   > 
   >     1. I/O模型的理解：用哪种通道，进行数据的发送和接收，很大程度决定了通信的性能
   >     
   >     2. Java能够支持的I/O模型：BIO、NIO、AIO
   >     
   >     3. Java BIO
   >     
   >        BIO，同步阻塞模型（传统阻塞模型）。
   >        
   >        服务器实现方式为：一个连接一个线程，即客户端有连接请求，服务器端必须开启一个线程进行处理，如果这个连接不做任何的事情，会造成不必要的线程开销
   >        
   >        因此对于高并发的情况，BIO模型有它的瓶颈
   >        
   >     4. Java NIO
   >     
   >        NIO，同步非阻塞模型。
   >        
   >        服务器实现方式：一个线程处理多个请求，即服务器端会用一个线程，线程中维护了Selector(选择器)，Selector会维护多个请求并对请求进行轮询，如果请求有I/O事件，就对该连接进行处理，这种就叫多路IO复用
   >        
   >        NIO的理论基础就是，当客户端和服务器端建立连接后，连接并不是时时刻刻都在活动状态(读写数据)
   >        
   >     5. Java AIO
   >     
   >        AIO，异步非阻塞模型(有人称为NIO.2)。
   >        
   >        AIO引入了异步通道的概念，采用了Proactor模式，简化了程序编写，有效的请求才会启动线程，AIO特点是先通过操作系统完成后再通知服务器端启动线程去处理。
   >        
   >        AIO适用于连接数多并且连接时间较长的应用，这里仅做了解
   >     
   > 3. BIO、NIO、AIO使用场景的分析
   > 
   >     1. BIO，适用于连接数较小且固定的架构，这种方式对服务器的资源要求很高，并发局限于应用中，在JDK1.4之前是唯一选择。好处是简单
   >     
   >     2. NIO，适用于连接数多且连接时间较短的架构，比如聊天服务器，弹幕系统，服务器之间的通信等。编程比较复杂，JDK1.4之后开始支持
   >     
   >     3. AIO，适用于连接数目多且连接时间较长的架构，比如相册服务器。编程复杂，JDK7后开始支持。
   >     
## 4. BIO的介绍
4. BIO的介绍
   > 1. 介绍
   > 
   >    BIO(Bloking I/O)，传统Java IO，相关的类全部在java.io包下
   >    
   >    可以通过线程池改善：改善的只是支持多客户连接，但是无法改善自身线程的个数
   >    
   > 2. BIO实例：参见D:/netty_code_home/netty_code
   > 
   > 3. BIO的问题
   > 
   >    1、当并发数据大时，需要大量的线程来处理连接，系统占用资源大；
   >    
   >    2、建立连接后，没有数据可读，会阻塞在Read操作上，造成线程资源浪费
   >    
## 5. NIO的介绍
5. NIO的介绍
   > 1. 介绍
   > 
   >    NIO(Non-bloking I/O)，是JDK提供的新的API。从JDK1.4之后出现，提供一系列改进的输入/输出新特性，统称NIO
   >    
   >    NIO相关的包，放在java.nio包下，是对原java.io包下很多类的改写
   >    
   > 2. NIO三大核心部分：NIO组成图解参见D:/JAVA教程百度云/尚硅谷java2020/尚硅谷Netty学习资料/课件/
   > 
   >     * Channel(通道)
   >     
   >     * Buffer(缓冲区)
   >     
   >     * Selector(选择器)
   >     
   > 3. NIO基本内容
   > 
   >     1. 是面向缓冲区或者说面向块编程的
   > 
   >        数据读取到一个稍后处理的缓冲区，需要时可以在缓冲区中前后移动，这使得它增加了处理过程中的灵活性，使用它可以提供非阻塞式的高伸缩网络
   >        
   >     2. Java NIO的非阻塞模式
   >     
   >        通过Channel和Buffer，使得线程从某个Channel中发送请求或者读取数据时，做到：读取数据，它只会读取到目前可用的数据，没有数据读取，不会保持线程阻塞，而是去完成其他任务，直到数据可读；写入数据，线程写入数据到通道，不用等待完全写入，线程就可以去完成其他任务
   >        
   >        Selector是基于事件驱动的，它会不断轮询所有的Channel；如果Channel检测到Buffer中有数据要读，Channel触发读事件，Selector检测到事件，处理该Channel的事务；
   >        
   >        通俗理解，NIO可以做到使用一个线程来对多个操作进行处理，因此它可以做到用少量的线程，处理大量请求
   >        
   >     3. Http2.0相比于Http1.0，使用了多路复用技术，做到同一个连接，并发处理多个请求，并且并发请求数量比Http1.0大好几个数量级
   >     
   > 4. Buffer初探。参见D:/netty_code_home/netty_code
   > 
   > 5. NIO和BIO的比较
   > 
   >     1. BIO以流的方式进行处理数据，NIO以块的方式处理数据。块I/O效率比流I/O效率高很多
   >     
   >     2. BIO是阻塞的，NIO是非阻塞的
   >     
   >     4. BIO是基于字节流和字符流进行操作的；而NIO基于Channel和Buffer进行操作，数组总是从Buffer到Channel或从Channel到Buffer。
   >     
   >     5. Selector可以监听多个Channel的事件(连接请求、数据到达等事件)，因此单个线程可以监听多个客户端的通道
   >     
## 6. Selector、Buffer、Channel简单的梳理
6. Selector、Buffer、Channel简单的梳理
   > 1. 一个Channel对应一个Buffer
   > 
   > 2. 一个Selector对应一个线程，因此一个线程对应多个Channel
   > 
   > 3. 程序切换到哪个Channel是由Channel中的事件(Event)决定，因此Event是关键概念
   > 
   > 4. Selector根据不同的Event在Channel之间切换
   > 
   > 5. Buffer本质是个双向的内存块
   > 
   > 6. 数据的输入输出通过Buffer控制，这是和BIO的重要区别。BIO必须要有两个流，一个输入流一个输出流；NIO的Buffer是双向的，只需要使用flip()进行切换
   > 
   > 7. Channel也是双向的
   > 
## 7. Buffer缓冲区
7. Buffer缓冲区
   
    > 1. Buffer基本介绍
    >
    >    Buffer本质上，是一个可以读写数据的内存块，可以理解成容器对象（含数组）。
    >
    >    Buffer提供了一组方法，可以更轻松的使用内存块
    >
    >    Buffer对象内置了一些机制，用来跟踪和记录缓冲区的变化情况
    >
    >    Channel提供从文件、网络读取数据的渠道，Buffer用来将数据读取和写入
    >
    > 2. 为什么说Buffer可以理解成容器对象（含数组）
    >
    >    通过查看继承了Buffer的IntBuffer、FloatBuffer、DoubleBuffer等，可以看到里面都有一个对应数据类型的、由final修饰的数组
    >    
    >    该数组用来存储真实数据
    >    
    > 3. Buffer类及其子类
    >
    >     1. Buffer类层级结构
    >     
    >         * Buffer类：抽象类，是NIO里的顶层父类。有七个具体子类，注意，Boolean类型没有对应的Buffer
    >     
    >             * ByteBuffer：存储byte数据到缓冲区。因为网络中所有数据都是以byte流传输，因此该Buffer使用最多
    >         
    >             * ShortBuffer：存储String数据到缓冲区
    >         
    >             * CharBuffer：存储char数据到缓冲区
    >         
    >             * IntBuffer：存储int数据到缓冲区
    >         
    >             * LongBuffer：存储长整形数据到缓冲区
    >         
    >             * DoubleBuffer：存储double数据到缓冲区
    >         
    >             * FloatBuffer：存储float数据到缓冲区
    >         
    >     2. Buffer类中，定义有四个属性，提供关于其所包含元素的信息。Buffer类的子类通过继承这四个属性的public方法，来间接继承该四个属性
    >     
    >         ```
    >         private int mark = -1； // 标记。默认为-1，一般不会修改	
    >         ```
    >         
    >         ```
    >         private int capacity; // 容量，允许存放的最大数据量。缓冲区创建时指定，永不可变
    >         ```
    >         
    >         ```
    >         private int limit; // 缓冲区当前的终点索引，即当前缓冲区的读写极限。读写操作，不允许在>=limit的位置进行。极限可以修改
    >         ```
    >         
    >         ```
    >         private int position = 0; // 位置，存放下一次要读、写元素的索引，默认指向arr[0]的位置。每次对缓冲区进行操作都会改变position的值，为下次读写做准备
    >         ```
    >         
    >     3. 使用IntBuffer的创建、赋值、反转、读取值，来体会上述四个属性的变化。参见ppt
    >     
    >     4. Buffer类中，重要的公共方法：
    >     
    >         ```
    >         public final int capacity(); // 返回该Buffer的最大容量
    >         
    >         public final int position(); // 返回此时，该Buffer的position位置
    >         
    >         public final Buffer position(int newPosition); // 设置此Buffer的位置
    >         
    >         public final int limit(); // 返回该Buffer的此时limit
    >         
    >         public final Buffer limit(int newLimit); // 设置此Buffer的limit
    >         
    >         public final Buffer flip(); // 反转此Buffer。方法体中逻辑是：将position重新置为0，将limit置为写入后最终的position，因此反转后才可以读取数据
    >         
    >         public final Buffer clear(); // 清除此Buffer。注意，只是让各个属性回到原来的状态，Buffer中的数据没有擦除，之后只是将旧数据覆盖而已。每次读取完数据，Buffer必须进行清除操作
    >         
    >         public final boolean hasRemaining(); // 告知当前位置，和limit之间，是否还有元素
    >         
    >         public abstract boolean isReadOnly(); // 告知此Buffer是否为只读缓冲区
    >         
    >         
    >         /**         JDK1.6后引入的API            **/
    >         public abstract boolean hasArray(); // 告知此Buffer，是否有可访问的底层实现数组
    >         
    >         pubilc abstract Object array(); // 返回此Buffer的底层实现数组
    >         ```
    >         
    >     5. ByteBuffer里重要的方法：因为ByteBuffer是之后最常用的Buffer，因此必须掌握常用的方法
    >     
    >         ```
    >         /** 常用静态方法 **/
    >         public static ByteBuffer allocateDirect(int capacity); // 以某个容量，创建一个ByteBuffer
    >         
    >         public static ByteBuffer allocate(int capacity); // 设置一个ByteBuffer的初始容量。一般创建普通的ByteBuffer就是使用该方法，给Buffer数组的每个Buffer设置初始容量也是使用该方法
    >         
    >         /** 常用方法 **、
    >         public abstract byte get(); // 从当前位置position，get数据。get完毕，position自动+1
    >         
    >         public abstract byte get(int index); // 指定从某个index上，get数据
    >         
    >         public abstract ByteBuffer put(byte b); // 从当前位置上put数据，put完毕，position自动+1
    >         
    >         public abstract ByteBuffer put(int index, byte b); // 往指定index位置，put数据
    >         ```
    >         
## 8. Channel通道
8. Channel通道
    > 1. 基本介绍
    >
    >     1. Channel类似于Java中的流，但是和流有些区别：
    >     
    >         1. Channel可以同时进行读写，流只能实现读或者写
    >         
    >         2. Channel可以异步读写数据
    >         
    >         3. Channel和Buffer之间可以进行数据交互，写入输入到Buffer或者从Buffer中读数据
    >         
    >     2. BIO中，流要么是输入流，要么是输出流，只能单向传输数据；NIO中，Channel是双向的，可以读也可以写
    >     
    >     3. Channel接口和重要的实现类
    >     
    >         1. Channel在Java中，是一个接口，存在于java.nio.channels包下
    >     
    >             ```
    >             public interface Channel extends Closeable{}
    >             ```
    >             
    >         2. Channel重要的实现类：
    >         
    >             * FileChannel：用于文件的读写
    >             
    >                 1. 介绍
    >             
    >                    FileChannel是抽象类，真正用到的是FileChannelImpl类
    >                    
    >                    用于对本地文件进行IO操作
    >                    
    >                 2. 常用的方法
    >             
    >                     ```
    >                     public int read(ByteBuffer dst); // 从通道读数据，放入Buffer
    >                     
    >                     public int write(ByteBUffer src); // 将缓冲区的数据，写入通道中
    >                     
    >                     public long transferForm(ReadableByteChannel src, long position, long count); // 从目标通道中，复制数据进入当前通道
    >                     
    >                     public long transferTo(long position, long count, WritableByteChannel target); // 将数据从当前通道读取出来，复制给目标通道
    >                     ```
    >             
    >             * DatagramChannel：用于UDP的读写
    >             
    >             * ServerSocketChannel类：用于TCP的读写
    >             
    >                 1. 介绍
    >             
    >                    ServerSocketChannel类是NetworkChannel类的子类，
    >                    
    >                    NetworkChannel类是Channel类的子类
    >                    
    >                    ServerSocketChannel类是一个抽象类，真正用到的是它的实现类ServerSocketChannelImpl类
    >                    
    >                 2. 使用
    >                 
    >                    服务端使用
    >                    
    >                    new InetSocketAddress(端口号)来创建一个端口
    >                    
    >                    ServerSocketChannel可以通过socket()方法，获取一个ServerSocket，然后通过后去的ServerSocket的bind(InetSocketAddress inetSocketAddress)方法对端口进行监听
    >                    
    >                    监听完成后，调用ServerSocketChannel的accept()方法，该方法返回SocketChannel，一旦服务端检测到连接，就会分配一个SocketChannel
    >             
    >             * SocketChannel：用于TCP的读写
    >             
    >                 1. 介绍
    >                 
    >                    SocketChannel类是NetworkChannel类的子类
    >                    
    >                    SocketChannel是一个抽象类，真正用到的是SocketChannelImpl类
    >     
    > 2. 实际案例，将Channel和Buffer结合。具体代码参见D:/netty_code_home
    >
    >     1. 往一个文件中写入一段话
    >     
    >         1. FileChannel和文件流的关系
    >         
    >            通过文件流的getChannel()方法获取到FileCHannel，可见FileChannel是文件流中的属性
    >            
    >         
    >     2. 将刚才写入的文件读到控制台
    >     
    >     3. 拷贝刚才的文件，放入项目下
    >     
    >         1. 拷贝时，因为不知道读取文件大小，因此要循环读取
    >     
    >         2. 每次读取前，必须对Buffer进行复位，调用clear()方法。
    >     
    >            因为如果文件很大，Buffer较小，一次读取后Buffer占满了，下次读取，因为没有复位，会无法继续往里面添加数据的，则写入Buffer字节数永远为0，死循环
    >         
    >     4. 拷贝文件，放入项目下。使用通道的transferTo()和transferFrom()方法，快速拷贝
    >     
## 9. 关于Buffer和Channel的注意事项和细节
9. 关于Buffer和Channel的注意事项和细节
    > 1. ByteBuffer支持类型化的put和get，例如putInt()、getInt()、putLong()、getLong()
    >
    >    使用类型化来put和get，put放入什么类型数据，必须用什么类型数据通过get来接收，否则可能会报```BufferUnderflowException```异常。
    >    
    >    ```BufferUnderflowException```异常说明：读取的长度超出了允许的长度
    >    
    > 2. 普通的Buffer可以转为只读Buffer
    >
    >    当一个Buffer中数据只读，可以将Buffer转为只读Buffer。只读Buffer不能使用put()放入数据，否则抛出```ReadBufferException```异常
    >    
    >    通过Buffer中的asReadOnlyBuffer()方法，就可以将一个普通Buffer转为一个只读Buffer。
    >    变成只读Buffer之后，可以使用循环，通过hasRemaining()方法判断，来将只读Buffer中的数据取出
    >    
    >     ```
    >     while(readBuffer.hasRemaining()) {
    >     	// 通过get()方法获取只读Buffer中数据，然后操作数据即可
    >     }
    >     ```
    >    
    > 3. NIO还提供了一个特殊的Buffer，他就是MappedByteBuffer。
    >
    >     1. MappedByteBuffer介绍
    >     
    >        MappedByteBuffer是ByteBuffer的子类
    >        
    >        它可以让文件直接在内存（不是堆内存，是其他内存空间）中直接进行修改，如何同步到文件由NIO自动处理，这是一个操作系统级别的修改
    >        
    >        MappedByteBuffer是抽象类，真正发挥作用的是DirectByteBuffer
    >        
    >     2. MappedByteBuffer好处：由于直接在内存修改文件，因此效率较高
    >       
    >     3. MappedByteBuffer的使用案例
    >        
    >         ```
    >         public static void main(String[] args) throws IOException {
    >                 // 使用RandomAccessFile对文件进行rw操作
    >                 RandomAccessFile randomAccessFile = new RandomAccessFile("d:\\file01.txt", "rw");
    >         
    >                 // 获取对应通道
    >                 FileChannel channel = randomAccessFile.getChannel();
    >                 
    >                 // 通过Channel获取MappedByteBuffer
    >                 /*
    >                  * map()方法的各个参数说明
    >                  * 第一个参数，代表使用的模式。FileChannel.MapMode.READ_WRITE说明是读写模式
    >                  * 第二个参数，代表直接修改的起始位置，从0开始计算。0代表起始位置
    >                  * 第三个参数，代表从起始位置开始算，允许映射到内存的大小，单位是字节。5代表5个字节
    >                  */
    >                 MappedByteBuffer mappedBuffer = channel.map(FileChannel.MapMode.READ_WRITE, 0, 5);
    >         
    >                 // 修改
    >                 mappedBuffer.put(0, (byte)'3'); // 修改第1个字节
    >                 mappedBuffer.put(4, (byte)'9'); // 修改第5个字节
    >         
    >                 // 修改成功，关闭流
    >                 randomAccessFile.close();
    >         
    >             }
    >         ```
    >         
    >         * map()方法的第三个参数，写数字n，就是从第二个参数m开始，包括m能修改几个字节。如果修改字节范围超出，会抛出```IndexOutOfBoundException```异常索引越界
    >     
    > 4. 前面，我们进行文件拷贝等，都是使用一个Buffer来进行。NIO支持通过多个Buffer（即Buffer数组）来完成读写操作。即Scattering分散和Gathering聚合
    >
    >     1. Scattering分散：将数据写入到Buffer，可以采用Buffer数组，依次写入
    >     
    >     2. Gathering聚合：从Buffer中取出数据时，可以采用Buffer数组，依次取出
    >     
    >     3. 验证Scattering分散和Gathering聚合：
    >     
    >         1. 服务端：
    >         
    >             ```
    >             public static void main(String[] args) throws IOException {
    >                     // 这里使用ServerSocketChannel和SocketChannel来做通道，这就涉及到网络
    >             
    >                     // 创建ServerSocketChannel进行监听
    >                     ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
    >                     // 创建监听网络端口地址
    >                     InetSocketAddress inetSocketAddress = new InetSocketAddress(7000);
    >                     // 绑定监听端口到Socket，并启动监听
    >                     serverSocketChannel.socket().bind(inetSocketAddress);
    >             
    >                     // 创建Buffer数组，并分配空间
    >                     ByteBuffer[] byteBuffers = new ByteBuffer[2];
    >                     byteBuffers[0] = ByteBuffer.allocate(5);
    >                     byteBuffers[1] = ByteBuffer.allocate(3);
    >             
    >                     // ServerSocketChannel等待客户端连接
    >                     SocketChannel socketChannel = serverSocketChannel.accept();
    >             
    >                     // 循环读取数据
    >                     int message = 8; // 假定从客户端接收8个字节
    >                     while (true) {
    >                         int byteRead = 0;
    >                         while (byteRead < message) {
    >                             // 分散写入
    >                             long read = socketChannel.read(byteBuffers);
    >                             byteRead += read; //累积读取的字节数
    >                             System.out.println("byteRead=" + byteRead);
    >                             // 使用Stream流的map()方法
    >                             // 将Buffer数组中的两个Buffer中的position和limit通过标准打印流打印出来
    >                             Arrays.asList(byteBuffers).stream()
    >                                     .map(byteBuffer -> "position=" + byteBuffer.position() + "；limit=" + byteBuffer.limit())
    >                                     .forEach(System.out::println);
    >                         }
    >             
    >                         // 一次读取完毕，反转Buffer
    >                         Arrays.asList(byteBuffers).forEach(Buffer::flip);
    >             
    >                         // 重新将数据显示到客户端
    >                         long byteWrite = 0;
    >                         while (byteWrite < message) {
    >                             // 聚合导出
    >                             socketChannel.write(byteBuffers);
    >                             byteWrite += 1;
    >                         }
    >             
    >                         // 将所有Buffer进行复位
    >                         Arrays.asList(byteBuffers).forEach(Buffer::clear);
    >             
    >                         System.out.println("byteRead=" + byteRead + ";byteWrite=" + byteWrite + ";message=" + message);
    >                     }
    >             
    >                 }
    >             ```
    >             
    >         2. 在cmd中，使用telnet来模拟客户端连接，验证分散和聚合。
    >         
    >             根据模拟客户端连接验证后，可以看到
    >         
    >             分散：往Buffer数组存放数据，数据会先存放满第一个Buffer，然后存到第二个Buffer
    >         
    >             聚合：从Buffer数组读取数据，会先读第一个Buffer，然后依次读所有的Buffer
    >         
    >     4. 使用Buffer数组的好处：提高读写效率
    >     
## 10. Selector选择器
10. Selector选择器
    > 1. 基本介绍
    >
    >     1. Selector，选择器，也叫多路复用器
    >     
    >     2. Java的NIO，用非阻塞的IO方式，可以用一个线程处理多个客户端的连接。想要实现Selector必不可少
    >     
    >     3. Selector类似于SpringCloud中的Eureka注册中心，Selector可以接受多个Channel通过事件注册到Selector上。
    >     
    >     4. Selector会轮询它上面注册的通道，检测是否有事件发生，如果有就获取对应事件，然后针对不同事件进行相应处理。因此，就可以做到一个单线程管理多个通道，也就是管理多个请求
    >     
    >     5. 使用Selector的好处：
    >     
    >        1、只有在读写事件发生时，才会读写，大大减少了系统的开销；并且不必为每个连接创建线程，不用维护多个线程
    >        
    >        2、避免了多线程之间，上下文切换的开销
    >        
    >     6. Selector的特点详解：
    >     
    >         1. Netty中的IO线程NioEventLoop聚合了Selector，因此可以同时并发处理成百上千个客户端连接
    >         
    >         2. 当线程从某客户端SocketChannel进行读写时，如果没有数据需要操作，线程可以进行其他操作
    >         
    >         3. 线程将非阻塞IO的空闲时间，用于对其他的Channel进行IO操作，因此一个线程可以管理多个输入Channel和输出Channel
    >         
    >         4. 因为读写操作非阻塞，因此可以充分的提升IO线程的运行效率，避免由于频繁的IO阻塞导致的线程挂起
    >         
    >         5. 一个IO线程就可以处理并发N个客户端连接和读写操作，从根本上提升了架构的性能、弹性伸缩能力、可靠性
    >     
    > 2. Selector类的介绍和相关方法
    >
    >     1. Selector是一个抽象类
    >     
    >     2. Selector类和常用的方法如下：
    >     
    >         ```
    >         public abstract class Selector implements Closeable {
    >         
    >         	public static Selector open(); // 得到一个选择器对象
    >         	
    >         	public int select(); // 阻塞监控所有注册的Channel
    >         	
    >         	public int select(long timeout); // 监控所有注册的Channel。参数从来设置超时时间
    >         	
    >         	public void wakeup(); // 唤醒selector。如果Selector调用select方法，处于阻塞状态，调用wakeup()方法，立即返回
    >         	
    >         	public int selectNow(); // 监控所有注册的Channel，如果此时没有IO操作的Channel，立即返回
    >         	
    >         	public Set<SelectionKey> selectedKeys(); // 从内部集合属性中，获取到全部的SelectionKey，并返回
    >         	
    >         	
    >         	
    >         }
    >         ```
    >         
    >         * select方法：select方法，有两个重载的形式
    >         
    >             1. select()方法：
    >             
    >                它其实是一个阻塞的方法。它会一直阻塞，直到该Selector中注册的所有Channel中，有各自关注的事件发生，就返回发生事件的Channel个数
    >             
    >             2. select(int timeout)方法：
    >             
    >                通过设置超时时间，select方法就变成非阻塞。
    >               
    >                select(int timeout)会阻塞timeout的时间来监听所有祖册的Channel，Channel没有关注的事件发生，经过timeout时间后，自动返回；如果有关注的事件，立刻返回
    >             
    >         * selectedKeys()方法：
    >         
    >            selectedKeys()方法返回该Selector监控的所有Channel的SelectionKey的集合
    >            
    >         * SelectionKey是什么，有什么用？
    >         
    >             1. SelectionKey是什么：SelectionKey是一个抽象类，它和Channel是一一对应关系
    >         
    >             2. SelectionKey用处：Selector可以通过遍历SelectionKey的Set集合，得到SelectionKey，先通过SelectionKey得到具体发生的事件（读、写），然后通过SelectionKey反向得到对应的Channel，然后对事件进行操作
    >             
## 11. NIO非阻塞网络编程原理分析图
11. NIO非阻塞网络编程原理分析图：
    > 1. Selector、SelectionKey、SocketChannel、ServerSocketChannel关系梳理图，参见视频
    >
    > 2. 总结关系梳理图：
    >
    >     1. ServerSocketChannel监听端口，并向Selector注册
    >     
    >     2. Selector监听，使用select方法。返回有事件发生的通道个数
    >     
    >     3. 进一步得到各个SelectionKey（有事件发生的Channel的SelectionKey）
    >     
    >     4. 遍历SelectionKey（有事件发生的Channel的SelectionKey）的Set集合，根据事件来做操作
    >     
    >         1. 当客户端连接事件发生时
    >     
    >            1. 通过SelectionKey反向获取ServerSocketChannel，然后得到对应的SocketChannel
    >     
    >            2. SocketChannel会调用register方法，将自己注册到Selector上
    >     
    >               * register方法是SocketChannel继承自父类的
    >     
    >                 1. SocketChannel继承了AbstractSelectableChannel抽象类，在AbstractSelectableChannel抽象类中，有一个register(Selector sel, int ops, Object att)。
    >     
    >                    ```
    >                    register(Selector sel, int ops, Object att)
    >                    -- 一般用于客户端连接时，ServerSocketChannel分配的SocketChannel的注册。此时该SocketChannel一般是读Channel，第二个参数选择SelectionKey.OP_READ；第三个参数可以传入一个Buffer，用来接收Channel读的数据。
    >                    ```
    >     
    >                 2. AbstractSelectableChannel抽象类继承自SelectableChannel抽象类，在SelectableChannel抽象类中，也有一个register(Selector sel, int pos)。一般这个register方法使用较多。参数1是Selector实例，参数2是会引起Selector关注的事件
    >     
    >                    * 参数2ops关注的事件有四种取值：
    >     
    >                      1. SelectionKey.OP_READ：读事件
    >     
    >                      2. SelectionKey.OP_WRITE：写事件
    >     
    >                      3. SelectionKey.OP_CONNECT：连接建立成功事件
    >     
    >                      4. SelectionKey.OP_ACCEPT：有客户端连接我们事件
    >     
    >            3. SocketChannel注册后，返回的SelectionKey，会放入Selector中的SelectionKey集合
    >     
    >         2. 当客户端读写事件发生
    >     
    >            1. 根据SelectionKey反向获取SocketChannel。
    >     
    >               * 通过SelectionKey类中的channel()方法，channel()方法返回值为SelectableChannel，利用多态，自然可以得到SocketChannel
    >            
    >            2. 利用得到的Channel完成业务处理
    >            
    >     
## 12. NIO快速入门
12. NIO快速入门
    > 1. 代码参见D:/netty_code_home/netty_code
    >
    > 2. 案例1：编写一个NIO入门案例，实现客户端和服务券之间简单通信（非阻塞）
    >
    >     1. 要编写两个程序，一个客户端，一个服务端
    >     2. ServerSocketChannel也是一个Channel，它在绑定监听端口后，也必须注册到Selector。SelectionKey.OP_ACCEPT建立连接事件，就是为ServerSocketChannel注册使用的
    >     3. 事件处理完毕，必须手动删除集合中处理完毕后的selection。因为以后肯定会多线程操作，不删除已经处理完的selection会造成问题
    >     4. 在Buffer类中，有一个wrap(byte[] bytes)方法，可以创建一个底层数组和参数数组大小相同的Buffer，并将参数中数组数据写入Buffer底层数组
    >     5. 必须设置通道为 非阻塞，才能向 Selector 注册。否则，会出现```java.nio.channels.IllegalBlockingModeException```异常。因此，ServerSocketChannel分配的SocketChannel也需要设置为非阻塞，否则无法注册
    >     
## 13. SelectionKey API
13. SelectionKey API
    > 1. 基本介绍
    >
    >     SelectionKey，表示了Selector和Channel的注册关系
    >
    > 2. SelectionKey中，有四个常量来表示注册关系
    >    
    >      ```
    >     public static final int OP_READ = 1 <<0;
    >     public static final int OP_WRITE = 1 <<2;
    >     public static final int OP_CONNECT = 1 <<3;
    >     public static final int OP_ACCEPT = 1 <<4;
    >     ```
    >     
    > 3. Channel在Selector注册成功后的SelectionKey存放在哪里？通过断点调试分析
    >
    >      1. Selector是抽象类，真正创建的是WindowSelectorImpl的实例
    >      
    >      2. 注册成功后的Channel，生成的SelectionKey，会被存储在Selector的keys属性中。keys成员属性是一个Set集合，专门用来存储SelectionKey。
    >      
    >          * Selector中有一个keys()方法，会返回keys成员属性，里面存放着所有注册过的Channel的SelectionKey
    >      
    > 4. SelectionKey中，重要的方法
    >
    >     ```
    >     Selector selector(); // 得到与之相关联的Selector
    >     
    >     SelectableChannel channel(); // 得到与之相关的Channel。具体的Channel可以强转
    >     
    >     final Object attachment(); // 得到与之关联的共享数据。例如Buffer，可以通过该方法获取
    >     
    >     SelectionKey interestOps(int ops); // 设置或改变Channel在Selector的关联事件
    >     
    >     final boolean isAccept(); // 是否为连接事件
    >     
    >     final boolean isReadable(); // 是否为可读事件
    >     
    >     final boolean isWritable(); // 是否为可写事件
    >     
    >     void cancle(); // 将该SelectionKey对应的Channel从Selector取消注册
    >     ```
    >     
## 14. ServerSocketChannel API
14. ServerSocketChannel API
    > 1. ServerSocketChannel用于监听新的客户端连接
    >
    > 2. ServerSocketChannel重要方法
    >
    >     ```
    >     static ServerSocketChannel open(); // 得到一个ServerSocketChannel
    >     
    >     final ServerSocketChannel bind(SocketAddress local); // 绑定本机监听端口
    >     
    >     final ServerSocketChannel configureBlocking(boolean block); // 设置该ServerSocketChannel为阻塞还是非阻塞。只有非阻塞才能在Selector注册
    >     
    >     SocketChannel accept(); // 接收客户端连接，并分配一个SocketChannel
    >     
    >     final SelectionKey register(Selector selector, int ops); // 注册ServerSocketChannel
    >     ```
    >     
## 15. SocketChannel API
15. SocketChannel API
    > 1. SocketChannel，网络IO通道。具体负责读写操作，NIO把Buffer的数据写入通道；或者把通道数据读入Buffer
    >
    > 2. 常用方法
    >
    >     ```
    >     static SocketChannel open(); // 创建一个SocketChannel
    >     
    >     final SelectableChannel configureBlocking(boolean block); // 设置阻塞或者非阻塞模式。如果要往Selector注册，必须要设置成非阻塞模式
    >     
    >     boolean connect(SocketAddress address); // 连接具体服务器的具体端口
    >     
    >     boolean finishConnect(); //如果上述连接失败，可以使用该方式重试连接
    >     
    >     int write(ByteBuffer src); // 给Channel写数据
    >     
    >     int read(ByteBuffer src); // 从Channel读数据到Buffer
    >     
    >     final SelectionKey register(Selector selector, int ops, Object att); // 注册该SocketChannel，最后一个参数用来共享数据。
    >     
    >     final void close(); // 关闭通道
    >     ```
    >     
## 16. NIO网络编程实例：群聊系统
16. NIO网络编程实例：群聊系统。代码参见D:/netty_code_home/netty_code
    > 1. 要求：
    > 
    >     1. 实现服务器端和客户端之间的简单通信（非阻塞）
    >     
    >     2. 实现多人群聊
    >     
    >     3. 服务器端，可以检测用户上线、离线，并实现转发消息功能
    >     
    >     4. 客户端，通过Channel可以无阻塞发送消息给其他用户，同时可以接受其他用户发送的消息
    >     
## 17. NIO与零拷贝
17. NIO与零拷贝
    > 1. 零拷贝基本介绍和引出的问题
    > 
    >     1. 零拷贝是网络编程的基础，很多的性能优化，都离不开零拷贝
    >     2. 零拷贝，指的是：读写过程中，只有DMA拷贝，不存在CPU拷贝
    >     3. 零拷贝在Java程序中，常用的有mmap(内存映射)和sendFile。在具体的OS中，它们两个是如何设计的呢？
    >     4. 在NIO中如何使用零拷贝？
    >     
    > 2. 传统IO读写文件数据
    > 
    >     1. 过程
    >     
    >         1. 创建文件
    >     
    >         2. 得到RandomAccessFile
    >     
    >         3. 建立Socket
    >     
    >         4. 使用Socket中的OutputStream，将文件的字节数组写入OutputStream
    >         
    >     2. 传统IO读写数据模型分析
    >     
    >         1. 通过DMA拷贝，将硬盘数据拷贝到内核的缓冲区
    >         
    >             * DMA拷贝：Direct Memory Access 拷贝，直接内存拷贝。不经过CPU
    >             
    >         2. 将内核缓冲区数据，拷贝到用户缓冲区
    >         
    >         3. 将用户缓冲区的数据，拷贝到Socket中的缓冲区
    >         
    >         4. 将Socket的缓冲区中的数据，通过DMA拷贝，拷贝到协议栈
    >         
    >         5. **综上，传统IO读写数据，必须经过4次拷贝，过程中有3次状态切换**
    >     
    > 3. mmap优化
    > 
    >     1. 介绍
    >     
    >        传统IO一次读或写，需要进行4次拷贝，效率太低
    >        
    >        mmap是内存映射优化，它可以将文件直接映射到内核缓冲区，同时，用户空间可以共享内核空间的数据。因此，可以减少内核空间到用户空间的拷贝次数
    >        
    >     2. mmap读写过程
    >     
    >         1. 硬件中文件，通过DMA拷贝到内核缓冲区
    >         
    >         2. 内核缓冲区因为和用户空间直接共享数据，因此不需要将内核缓冲区数据拷贝到用户缓冲区。用户修改完毕后，直接将内核缓冲区的数据，通过CPU拷贝到Socket缓冲区
    >         
    >         3. Socket缓冲区数据，DMA拷贝到协议栈
    >         
    >         4. **综上，文件数据，mmap读写，拷贝次数为3次，过程中状态改变次数为3次**
    >         
    >     3. mmap并没有真正实现零拷贝
    >     
    > 4. sendFile
    > 
    >     1. 介绍
    >     
    >        Linux2.1提供的sendFile函数
    >        
    >        基本原理为：数据不经过用户态，直接从内核缓冲区进入到Socket缓冲区。因为和用户态无关，减少了一次状态切换。
    >        
    >     2. Linux2.1版本的sendFile函数读写模型
    >     
    >         1. 硬盘文件数据，DMA拷贝到内存缓冲区
    >         
    >         2. 内存缓冲区数据直接CPU拷贝到Socket缓冲区
    >         
    >         3. Socket缓冲区数据，通过DMA拷贝，拷贝到协议栈
    >         
    >         4. **综上，sendFile读写，经过3次拷贝，过程中2次状态切换**
    >     
    > 5. sendFIle优化
    > 
    >     1. 介绍
    >     
    >        LInux2.4版本，做了修改，避免了内核缓冲区到Socket缓冲区的拷贝操作，直接拷贝到协议栈。
    >        
    >     2. Linux2.4版本的sendFile函数读写模型
    >     
    >         1. 硬盘文件数据，DMA拷贝到内存缓冲区
    >         
    >         2. 内存缓冲区，经过DMA拷贝，拷贝到协议栈。只有很少的信息从内存缓冲区CPU拷贝到Socket缓冲区，然后通过Socket缓冲区经过DMA拷贝到协议栈，可以忽略
    >         
    >         3. **综上，sendFile读写，经过2次拷贝，2次状态切换**
    >         
    >     3. Linux2.4版本的sendFile，基本不经过CPU拷贝，实现了真正的零拷贝
    >     
    > 6. 零拷贝再次理解
    > 
    >     1. 零拷贝从操作系统来讲，不经过CPU拷贝
    >     
    >     2. 零拷贝不仅数据复制更少，而且带来其他性能优势：更少的上下文切换、更少的CPU缓存伪共享、无CPU计算和校验
    >     
    > 7. mmap和sendFile总结
    > 
    >     1. mmap适合小数据量的读写，sendFile适合大数据量的读写
    >     
    >     2. mmap要4次拷贝3次切换，sendFile3次拷贝2次切换
    >     
    >     3. sendFile实现了真正的零拷贝，mmap没有
    >     
## 18. 零拷贝的实际案例
18. 零拷贝的实际案例
    > 1. 如何在NIO中，使用零拷贝。
    > 
    >    NIO中，零拷贝是通过transferTo()方法来实现
    >    
    > 2. transferTo()方法，在Linux下可以直接完成零拷贝任何文件；在Windows下，一次transferTo()，只能零拷贝8M的文件，需要分段传输文件，并且要注意传输时的位置
    > 
## 19. AIO简单了解
19. AIO简单了解
    > 1. 基本介绍
    > 
    >    AIO，Asychronous I/O，异步不阻塞IO，是在JDK7引入的。
    >    
    >    进行JavaIO编程时，常用两种模式：Reactor和Proactor
    >    
    >    Java的NIO就是Reactor，即事件驱动的IO处理
    >    
    >    Java的AIO就是Proactor，AIO引入了异步通道的概念，同时使用Proactor，有效的请求才会启动线程。特点是：先通过操作系统处理请求，然后通知服务器端启动线程处理请求，适用于连接多且连接时间长的请求。
    >    
    >    目前AIO没有广泛应用，对AIO的详细介绍，参考```<<Java新一代网络编程模型AIO原理及Linux系统AIO介绍>>```
    >    
## 20. Netty
20. Netty
    > 1. 基本介绍
    > 
    >     1. 为什么有Java的NIO，还需要使用Netty框架
    >     
    >        1、NIO的库和API使用繁杂，很麻烦，需要熟练掌握NIO三大核心组件
    >        
    >        2、需要熟悉Java多线程，因为NIO涉及到了Reactor模式，必须对NIO和多线程都很熟悉，才能写出高质量的NIO程序
    >        
    >        3、开发工作量和难度都很大。例如客户端的断开重连、网络闪断、半包读写、网络拥堵等问题
    >        
    >        4、JDK NIO的BUG：例如最著名的Epoll BUG，会导致Selector轮询，最终导致CPU100%，直到JDK7该BUG依然存在
    >        
    >     2. Netty好处
    >     
    >        1、是对JDK自带的NIO的API进行的封装，解决NIO自身的问题
    >        
    >        2、设计优雅
    >        
    >        3、使用方便
    >        
    >        4、高性能，吞吐量高，延迟更低，资源消耗少，最小化不必要的内存复制
    >        
    >        5、安全，提供了全套的SSL和StartTLS支持
    >        
    >        6、社区活跃，版本迭代周期短
    >        
    >     3. 如今使用最多的是Netty4.xx版本
    >     
### 1. Netty架构设计
1. Netty架构设计
    > 1. 线程模型基本介绍：
    >
    >     1. 现存的线程模型：
    >     
    >         * 传统I/O服务模型
    >         
    >         * Reactor模式
    >         
    >     2. 根据Reactor的数量和处理线程池线程数量不同，Reactor模式有3种典型实现：
    >     
    >         * 单Reactor单线程
    >         
    >         * 单Reactor多线程
    >         
    >         * 主从Reactor多线程
    >         
    >     3. Netty线程模式：主从Reactor多线程基础上，做了一定的改进，
    >     
    > 2. Reactor模式：Reactor模式，有人叫反应器模式，有人叫分发者模式，有人叫通知者模式
    >     1. 介绍
    >     
    >         1. 基于了I/O复用模型：多个连接公用一个阻塞对象，应用程序只要在一个阻塞对象等待，无序阻塞等待所有的连接。任意一个连接有数据处理，操作系统通知应用程序，线程从阻塞状态返回，开始处理业务
    >         
    >         2. 基于线程池复用资源：不必为每个连接创建线程，连接完成后的业务分配线程进行处理，一个线程可以处理多个连接的业务
    >         
    >     2. Reactor模式核心组成
    >     
    >         1. Reactor：在一个单独线程运行，只负责监听和分发事件。类似于接线员，只接受请求，然后将请求交给具体的线程
    >         
    >         2. Handler：事件处理器，真正处理具体的事务
    >         
    >     3. 单Reactor单线程模型
    >     
    >         1. 模型图，参见视频
    >         
    >         2. 之前的NIO群聊，就是单Reactor单线程
    >         
    >         3. 服务器端，使用一个线程，完成了所有的连接、读、写等操作。优点是简单明了；缺点是如果多客户端连接，无法支撑
    >         
    >         4. 该模型用于客户端数量有限，业务处理非常快速的场景。例如Redis处理业务复杂度为O(1)的情况下就会使用
    >         
    >     4. 单Reactor多线程模型
    >     
    >         1. 模型图，参见视频
    >         
    >         2. 优点：
    >         
    >            发挥了多核CPU的处理能力；
    >         
    >         3. 缺点：
    >         
    >         1、多线程的数据共享和访问比较复杂，
    >         
    >         2、Reactor处理请求是单线程运行的，在高并发应用场景容易出现瓶颈
    >         
    >     5. 主从Reactor多线程模型
    >     
    >         1. 模型图，参见视频
    >         
    >         2. 优点：
    >         
    >            1、父子线程之间，数据交互职责明确，父线程只接受连接，子线程完成后续业务；
    >            
    >            2、父子线程之间，数据交互简单，父线程只用把连接传给子线程，子线程无需返回任何数据给主线程
    >            
    >         3. 缺点：
    >         
    >            编程复杂度较高
    >            
    >         4. 主从Reactor多线程使用实例：
    >         
    >            Nginx使用到了主从Reactor多线程模型
    >            
    >            Memcached主从多线程
    >            
    >            Netty基于主从多线程模型
    >         
    >     6. Reactor模式的优点
    >     
    >        1、响应快，不必为单个同步时间所阻塞，虽然Reactor本身仍然是同步的
    >        
    >        2、最大程度的避免了复杂的多线程和同步问题，并且避免了多线程/进程的开销
    >        
    >        3、可扩展性好，可以方便的通过增加Reactor实例个数来重复利用CPU资源
    >        
    >        4、复用性更好。Reactor模式本身与具体的事件处理逻辑无关，具有高复用性
    >     
    > 3. Netty模型
    >
    >     1. 模型图，参见视频
    >     
### 2. Netty快速入门案例：TCP服务
2. Netty快速入门案例：TCP服务
   
    > 1. 要求：通过Netty搭建TCP一对一服务
    >
    >    1. 自定义Handler给Pipeline使用
    >    
    >       Pipeline中提供了许多可以使用的Handler，也可以使用我们自定义的Handler
    >       
    >       自定义的Handler必须继承Netty规定好的某个Handler，才能被Pipeline使用
    >       
    >    2. 自定义的Handler中，有一些要重写的方法。
    >    
    >        1. Server端的Handler，需要重写的方法：
    >    
    >            ```
    >            /**
    >             * 当通道有数据可读时，执行该方法
    >             * ChannelHandlerContext是一个上下文对象，含有Pipeline、SocketChannel、客户端连接 地址等信息
    >             * Object 是客户端发送的数据，默认是Object，根据实际需要做类型转换
    >             */
    >            @Override
    >            public void ChannelRead(ChannelHandlerContext ctx, Object msg) throws Exception {}
    >            ```
    >        
    >            ```
    >            /**
    >             * 数据读取完毕后，给客户端回送消息
    >             */
    >            @Override
    >            public void channelReadComplete(ChannelHandlerContext ctx) throws Exception {}
    >            ```
    >        
    >            ```
    >            /**
    >             * 处理异常的方法。一般出现异常，必须关闭通道
    >             */
    >             @Override
    >             public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {}
    >            ```
    >           
    >        2. Client端的Handler，要重写的方法：
    >        
    >            ```
    >            /**
    >             * SocketChannel就绪，触发该方法
    >             */
    >             @Override
    >             public void channelActive(ChannelHandlerContext ctx) throws Exception {}
    >            ```
    >            
    >            ```
    >            /**
    >             * 当通道有数据可读时，执行该方法
    >             */
    >             @Override
    >             public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {}
    >            ```
    >            
    >            ```
    >            /**
    >             * 异常发生，触发该方法
    >             */
    >             @Override
    >             public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {}
    >            ```
    >            
### 3. 根据Netty入门案例，对Netty进行源码分析
3. 根据Netty入门案例，对Netty进行源码分析
    > 1. NioEventLoopGroup含有的子线程（NioEventLoop）个数为：默认值=实际CPU核心数 * 2
    > 
    > 2. Server端创建的两个NIOEventLoopGroup对象，它们有children属性，children属性存放NIOEventLoop的集合。
    > 
    > 3. 每一个NioEventLoop中，有独立的Selector、taskQueue、executor
    > 
    > 4. Pipeline、SocketChannel、ChannelHandlerContext关系
    > 
    >     * ChannelHandlerContext：
    >     
    >        实际的对象是DefaultChannelHandlerContext
    >        
    >        ChannelHandlerContext是一个重量级的对象，里面有有Handler、Pipeline、Channel、executor、EventLoop（属于哪一个EventLoop）等大量的信息
    >        
    >     * Pipeline和Channel：
    >     
    >        Pipeline实际的类型为DefaultChannelPipeline，Channel类型为NioSocketChannel
    >        
    >        Pipeline和Channel是你中有我，我中有你的关系
    >        
    >        Pipeline底层是双向链表的结构
    >        
### 4. TaskQueue任务队列
4. TaskQueue
    > 1. 介绍
    >
    >    比如，我们有一个非常耗时的业务，如果同步执行，会造成Pipeline的阻塞
    >    
    >    因此，我们可以找到该Channel对应的NioEventLoop，然后找到对应的TaskQueue，将耗时任务放入TaskQueue
    >    
    > 2. TaskQueue中Task的3种典型应用场景
    >
    >     1. 用户程序自定义的普通任务
    >     
    >         * 操作步骤：
    >         
    >             ```
    >             // 找到对应的NioEventLoop，定义任务，交给对应的TaskQueue。TaskQueue会轮流执行耗时任务
    >             // 第一个耗时任务
    >             ctx.channel().eventLoop().execute(new Runnable() {
    >             	@Override
    >             	public void run() {
    >             		// 耗时的业务。这里用Thread.sleep(10 * 1000)模拟
    >             		Thread.sleep(10 * 1000);
    >             		System.out.println("耗时业务1执行完毕");
    >             	}
    >             });
    >             // 第二个耗时任务
    >             ctx.channel().eventLoop().execute(new Runnable() {
    >             	@Override
    >             	public void run() {
    >             		// 耗时的业务。这里用Thread.sleep(20 * 1000)模拟
    >             		Thread.sleep(20 * 1000);
    >             		System.out.println("耗时业务2执行完毕");
    >             	}
    >             });
    >             ```
    >         
    >     2. 用户自定义定时任务：
    >     
    >         * 特点：
    >     
    >           定时任务不会放在TaskQueue中，而是会放在ScheduleTaskQueue中
    >     
    >           定时任务和普通任务一起，执行顺序都是按照任务先后创建来执行的
    >     
    >         * 操作步骤：
    >     
    >           ```
    >           // schedule()方法的三个参数分别为，任务，延时时间长度，延时时间单位
    >           ctx.channel().eventLoop().schedule(new Runnable() {
    >           	@Override
    >           	public void run() {
    >           		Thread.sleep(20 * 1000);
    >           		System.out.println("延时任务执行完成")
    >           	}
    >           }, 2, TimeUtil.SECONDS);
    >           ```
    >     
    >     3. 非当前Reactor线程，调用Channel中的各种方法
    >     
    >        * 介绍
    >     
    >           例如在推送系统的业务线程中，根据用户标识，找到对应的Channel引用，然后利用Write类方法将推送消息推送给用户，就是这种场景。
    >          
    >           最终Write会提交到任务队列，然后异步消费
    >          
    >        * 思路：
    >        
    >           只要我们将每个客户连接后的SocketChannel统一维护，之后就可以在别的Reactor线程中，使用这些SocketChannel
    >           
    >          给Pipeline设置处理器的initChannel(SocketChannel ch)方法，每个客户端建立连接后都会执行。因此，我们可以利用这一点，获取每个用户的SocketChannel并统一维护。
    >          
### 5. Netty模型再说明
5. Netty模型再说明
    > 1. Netty抽象出两组线程池，bossGroup专门用来接收客户端连接，workerGroup专门用来网络读写
    > 
    > 2. NioEventLoop表示一个循环执行处理任务的线程，每个NioEventLoop都有一个 Selector，用于监听绑定在该Selector的Channel
    > 
    > 3. NioEventLoop内部采用了串行化设计，从```消息读 -> 解码 -> 处理 -> 编码 -> 发送```始终只用一个IO线程NioEventLoop执行
    > 
    > 4. NioEventLoopGroup包含了多个NioEventLoop，数量可以指定，也可以使用默认数量
    > 5. NioEventLoop中有一个Selector，一个TaskQueue；每个NioEventLoop的Selector可以注册多个Channel；每个Channel绑定到唯一的NioEventLoop；每个Channel绑定唯一的Pipeline
    > 
### 6. 异步模型
6. 异步模型
    > 1. 异步的概念
    >
    >    异步和同步相对，当一个异步过程调用后，调用者不能立即得到结果；在实际处理这个调用的组件完成后，通过状态、通知、回调等手段通知调用者
    >    
    > 2. Netty中的I/O操作是异步的，包括Bind、Write、Connect等操作会简单返回一个ChannelFuture。需要在bind方法、connect方法等后通过链式编程添加.sync()方法来启动异步。
    >
    > 3. 调用者无法直接获得结果，而是通过Future-Listner机制，用户可以方便的主动获取或者通过通知机制得知IO操作结果
    >
    > 4. Netty的异步模型建立在future和callback上。callback就是回调，重点了解future。
    >
    > 5. Future思想：假设有一个方法fun，它的计算过程可能会非常耗时，程序在此阻塞等待显然不合适，因此，可以在调用fun时立刻返回一个Future，后序通过Future来监控fun的处理过程
    >
    > 6. Future说明：
    >
    >     1. 表示异步的执行结果，可以通过它提供的方法，来检测执行是否完成，比如检索计算等
    >     
    >     2. ChannelFuture
    >     
    >         ChannelFuture是一个接口
    >         
    >         我们可以添加监听器，当监听的事件发生时，就会通知到监听器
    >     
    > 7. Future-Listner机制
    >
    >     1. 当Future对象刚刚创建，处于非完成状态，调用者可以通过返回的ChannelFuture来获取操作时的状态，注册监听函数来执行完成后面的操作
    >     
    >     2. 常见的操作有
    >     
    >         * 通过isDone来判断当前操作是否完成
    >         
    >         * 通过isSuccess来判断当前操作是否成功
    >         
    >         * 通过getCause来获取已经完成操作失败的原因
    >         
    >         * 通过isCancelled方法来判断以完成的当前操作是否被取消
    >         
    >         * 通过addListner来注册监听器，监控我们关心的事件
    >         
    >     3. 举例说明：
    >     
    >         ```
    >         // 启动服务器，并绑定端口。异步
    >         ChannelFuture cf = bootstrap.bind(8900).sync();
    >         
    >         // 注册监听器，监听关心的事件
    >         future.addListner(new ChannelFutureListner() {
    >         	@Override
    >         	public void oprationComplete(ChannelFuture future) throws Exception {
    >         		if (cf.isSuccess()) {
    >         			System.out.println("监听端口成功");
    >         		} else {
    >         			System.out.println("监听端口失败");
    >         		}
    >         	}
    >         });
    >         ```
    >         
### 7. 快速入门案例：Http服务
7. 快速入门案例：Http服务
    > 1. 要求：Netty服务器在8900端口监听，浏览器发送```http://localhost:8900/```请求，服务器发送消息给浏览器，并能对一些特定资源进行过滤
    > 
### 8. Netty核心组件
8. Netty核心组件
    > 1. Bootstrap和ServerBootstrap
    >
    >     1. 介绍
    >     
    >        Bootstrap意思是引导，一个Netty应用通常由Bootstrap开始，主要作用是配置整个Netty程序，串联各个组件。
    >        
    >        Netty中，Bootsrap类是客户端程序的启动引导类；ServerBootstrap是服务器端程序的启动引导类
    >        
    >     2. 常见的方法：
    >     
    >         ```
    >         public ServerBootstrap group(EventLoopGroup parentGroup, EventLoopGroup childGroup); // 用来配置服务器端的两个NioEventLoopGroup
    >         
    >         public B group(EventLoopGroup group); // 用来配置客户端的NioEventLoopGroup
    >         
    >         public B channel(Class<? extends C> channelClass); // 用来设置通道实现。服务器端是NioServerSocketChannel.class, 客户端是NioSocketCahnnel.class
    >         
    >         public <T> B option(ChannelOption<T> option, T value); // 用来给NioServerSocketChannel 添加配置
    >         
    >         public <T> ServerBootstrap childOption(ChannelOption<T> childOption, T value); // 用来给接收到的NioSocketChannel来添加配置
    >         
    >         public ServerBootstrap childHandler(ChannelHandler childHandler); // 用于workerGroup，用来设置业务处理类（自己定义的Handler）
    >         
    >         public ServerBootstrap handler(ChannelHandler handler); // 用于给bossGroup中，设置Handler
    >         
    >         public ChannelFuture bind(int inetPort); // 用于服务器端，设置占用的端口号
    >         
    >         public ChannelFuture connect(String inetAddress, int inetPort); // 用于客户端，设置连接的服务器端IP和端口
    >         ```
    >     
    > 2. Future和ChannelFuture
    >
    >     1. 介绍
    >     
    >        Netty中所有的IO操作都是异步的，无法立刻得知消息是否被正确处理，因此需要使用Future-Listner机制来监听
    >        
    >     2. 常用的方法
    >     
    >         ```
    >         Channel channel(); // 返回当前正在执行IO操作的Channel
    >         
    >         ChannelFuture sync(); // 等待异步操作执行完成
    >         ```
    >     
    > 3. Channel
    >
    >     1. 介绍
    >        
    >        是Netty进行网络通信的组件，能够用于网络IO操作
    >        
    >     2. 通过Channel可以获知当前网络连接的通道状态
    >     
    >     3. 通过Channel可以获得网络连接的配置参数（例如接受缓冲区的大小）
    >     
    >     4. Channel提供异步的网络IO操作（例如建立连接，读写，绑定端口等）
    >     
    >     5. 调用Channel进行IO操作，立即返回一个ChannelFuture
    >     
    >     6. 不同协议、不同阻塞类型的连接，Netty帮助我们封装了不同的具体Channel，常用的有：
    >     
    >         ```
    >         NioSocketChannel -- 异步的客户端Tcp Socket连接
    >         
    >         NioServerSocketChannel -- 异步的服务器端Tcp Socket连接
    >         
    >         NioDatagramChannel -- 异步的Udp 连接
    >         
    >         NioSctpChannel -- 异步的Sctp连接
    >         
    >         NioSctpServerChannel -- 异步的Sctp服务器端连接
    >         ```
    >     
    > 5. Selector
    >
    >     1. 介绍
    >     
    >        Netty基于Selector对象实现IO多路复用，通过Selector一个线程管理多个Channel
    >        
    >     2. 当向一个Selector注册Channel后，Selector内部的机制可以自动不断的轮询注册的Channel，监听是否有就绪的IO事件（例如可读、可写、网络连接完成等事件），这样就可以简单的通过一个Selector线程高效管理多个Channel
    >     
    > 6. ChannelHandler及其实现类
    >
    >     1. 介绍
    >     
    >        ChannelHandler是一个接口，用来处理IO事件或者拦截IO操作，并将其转发到其对应的Pipeline中的下一个处理程序
    >        
    >        ChannelHandler并没有提供很多的方法
    >        
    >     3. ChannelHandler及其实现类。我们主要关注最重要的几个子接口和实现类
    >     
    >         * ChannelInboundHandler：ChannelHandler子接口，用来处理入站IO事件	
    >         
    >             * ChannelInboundHandlerAdaptor：适配器类，用来处理入站IO事件。
    >             
    >         * ChannelOutboundHandler：ChannelHandler子接口，用来处理出站IO事件
    >         
    >             * ChannelOutboundHandlerAdaptor：适配器类，用来处理出站IO事件
    >             
    >         * ChannelDuplexHandler：用来处理出站和入站事件
    >         
    >     4. 什么是入站和出站
    >     
    >        Pipeline就相当于站
    >        
    >        Pipeline中提供了一个Handler链容器
    >        
    >        以客户端为例，如果数据从客户端流入服务器端，叫出站；从服务器端流向客户端，叫入站
    >        
    >     5. 我们通常需要自定义一个Handler类，然后继承ChannelInboundHandlerAdaptor，然后重写业务方法。一般按照需求重写下列方法：
    >     
    >         ```
    >         // 1. 通道活跃事件
    >         @Overide
    >         public void channelActive(ChannelHandlerContext ctx) throws Exception {}
    >         
    >         // 2. 通道读取数据事件
    >         @Overide
    >         public void channelRead(ChannelHandlerContext ctx) throws Exception {}
    >         
    >         // 3. 通道读取数据完毕事件
    >         @Overide
    >         public void channelReadComplete(ChannelHandlerContext ctx) throws Exception {}
    >         
    >         // 4. 通道发生异常事件
    >         @Overide
    >         public void exceptionCaught(ChannelHandlerContext ctx) throws Exception {}
    >         
    >         // 5. 通道注册事件
    >         @Overide
    >         public void channelRegistered(ChannelHandlerContext ctx) throws Exception {}
    >         
    >         // 6. 通道注销事件
    >         @Overide
    >         public void channelUnregistered(ChannelHandlerContext ctx) throws Exception {}
    >         
    >         // 7. 该Handler被加入Pipeline事件。连接建立，该方法第一个被调用
    >         @Overide
    >         public void handlerAdded(ChannelHandlerContext ctx) throws Exception {}
    >         
    >         // 8. 该Handler从Pipeline剔除事件
    >         @Overide
    >         public void handlerRemoved(ChannelHandlerContext ctx) throws Exception {}
    >         ```
    >     
    > 7. Pipeline和ChannelPipeline：ChannelPipeline是一个重点
    >
    >     1. ChannelPipeline介绍
    >     
    >        ChannelPipeline是Handler集合，它负责处理和拦截inbound入站和outbound出站的时间和操作，相当于一个贯穿了Netty的链。
    >        
    >        ChannelPipeline实现了一种高级形式的拦截过滤模式，使用户可以完全控制事件的处理方式以及Channel中各个ChannelHandler如何交互
    >        
    >     2. ChannelPipeline和ChannelHandlerContext关系
    >     
    >        ChannelPipeline中维护了一个由ChannelHandlerContext组成的双向链表
    >        
    >        每个ChannelHandlerContext关联了一个ChannelHandler
    >        
    >     3. ChannelPipeline中常用的方法
    >     
    >         ```
    >         ChannelPipeline addFirst(ChannelHandler... handlers) -- 把一个业务处理类(Handler)添加到链中第一个位置
    >         
    >         ChannelPipeline addLast(ChannelHandler... handlers) -- 把一个业务处理类(Handler)添加到链中最后一个位置
    >         ```
    >         
### 9. Netty核心组件(二)
9. Netty核心组件(二)
    > 1. ChannelHandlerContext
    >
    >     1. 保存了Handler相关的所有上下文信息，同时关联一个ChannelHandler对象
    >     
    >     2. ChannelHandlerContext中，包含一个具体的ChannelHandler，同时ChannelHandlerContext中也绑定了对应的Pipeline和Channel的信息，方便ChannelHandler调用
    >     
    >     3. ChannelHandlerContext常用方法
    >     
    >         ```
    >         ChannelFuture close(); // 关闭通道
    >         
    >         ChannelFuture wirteAndFlush(Object msg); // 将数据写入到ChannelPipeline中，当前ChannelHandler的下一个ChannelHandler，开始处理（数据出站）
    >         ```
    >     
    > 2. ChannelOption
    >    
    >     1. Netty在创建Channel实例后，一般都需要设置ChannelOption参数。一般是通过BootStrap或ServerBootstrap中的方法来设置
    >     
    >     2. ChannelOption参数如下：
    >     
    >         ```
    >         ChannelOption.SO_BACKLOG -- 对应TCP/IP协议的listen函数中的backlog参数，用来初始化服务器可连接队列的大小。服务器端处理客户端请求是按照顺序来处理，一次处理一个，剩下的放入等待队列中，backlog参数指定等待队列大小
    >         
    >         ChannelOption.SO_KEEPALIVE -- 一直保持连接活动状态
    >         ```
    >     
    > 3. EventLoopGroup和其实现类NioEventLoopGroup
    >
    >     1. EventLoopGroup是一组EventLoop的抽象。Netty为了更好利用多核CPU资源，一般会有多个EventLoop同时工作，每一个EventLoop都维护了一个Selector实例
    >     
    >     2. EventLoopGroup提供了next接口，可以按照组中的规则获取其中一个EventLoop来处理任务。在Netty服务器编程中，我们一般需要两个EventLoopGroup来完成工作，即bossGroup和workerGroup
    >     
    >     3. 通常一个服务器端口，即一个ServerSocketChannel对应一个Selector和一个EventLoop。bossGroup负责接受客户端的连接并将SocketChannel交给workerGroup来进行IO操作。
    >     4. EventLoopGroup常用的方法
    >     
    >         ```
    >         public NioEventLoopGroup(); // 构造方法
    >         
    >         public Future<?> shutdownGracefully(); // 断开连接，关闭线程。
    >         ```
    >     
    > 4. Unpooled类
    >
    >     1. Unpooled类是Netty提供的，专门用来处理缓冲区（Netty提供的缓冲区）的工具类。
    >     
    >     2. 常用方法
    >     
    >         ```
    >         public static ByteBuf copiedBuffer(CharSequence string, Charset charset); // 通过给定的字符串和字符编码，反会有一个ByteBuf对象。ByteBuf对象类似于NIO的ByteBuffer对象，但是还是有些区别
    >         ```
    >         
    >     3. Netty的ByteBuf对象，不需要和NIO的ByteBuffer一样，使用flip进行翻转。因为ByteBuf底层维护了readIndex和writeIndex两个属性。
    >     
    >     4. readIndex表示下一次读取位置，从0开始；writeIndex表示下一次写入位置，从0开始。这两个属性将ByteBuf分为三个区域，第一个区域0--readIndex，表示已经读取过的区域；第二个区域readIndex--writeIndex，表示可读取的区域；第三个区域writeIndex--ByteBuf.capacity()，表示可以写入的区域。
    >     
### 10. Netty群聊系统
10. Netty群聊系统
    
    > 1. 要求：能做到客户端消息经过服务器端转发到其他客户端
    >
    > 2. 服务器端
    >
    >    1. 为了转发到其他客户端，我们需要一个存储有所有客户端Channel的Channel组
    >
    >       使用Netty提供的Channel组，用它来管理所有的Channel。因为该Channel组所有用户的Handler共享，因此使用static修饰
    >    
    >        ```
    >        // GlobalEventExecutor.INSTANCE 是一个全局的事件执行器，是一个单例
    >        private static ChannelGroup channels = new DefaultChannelGroup(GlobalEventExecutor.INSTANCE);
    >        ```
    >    
    >     3. 我们需要在每个客户端和服务器端建立连接后，将客户端的Channel放入Channel组，并且提示其他客户端有人加入群聊
    >
    >        我们重写handlerAdded方法，该方法是第一个被调用的，连接建立就立即执行该方法
    >
    >         ```
    >        /**
    >             * handlerAdded方法，是第一个被调用的，连接建立就立即执行该方法
    >             * @param ctx
    >             * @throws Exception
    >             */
    >     	     @Override
    >            public void handlerAdded(ChannelHandlerContext ctx) throws Exception {
    >    
    >                Channel channel = ctx.channel();
    >    
    >                // 将该Channel加入聊天的消息，发送给其他客户端
    >                channels.writeAndFlush("客户端[" + channel.remoteAddress() +"]加入聊天\n");
    >    
    >                // 将当前Channel放入Channel组中
    >                channels.add(channel);
    >    
    >            }
    >         ```
    >
    >     4. 我们需要在客户端断开连接后，将客户端Channel从Channel组中剔除，并且提示其他客户端
    >
    >         ```
    >         /**
    >              * 客户端断开连接触发。该事件触发，会自动将Channel组中该客户端的Channel剔除，不用手动操作
    >              * @param ctx
    >              * @throws Exception
    >              */
    >             @Override
    >             public void handlerRemoved(ChannelHandlerContext ctx) throws Exception {
    >     
    >                 // 给其他客户端提示离线信息
    >                 channels.writeAndFlush("客户端[" + ctx.channel().remoteAddress() + "]已经离线\n");
    >     
    >             }
    >         ```
    >         
#### 1. Netty群聊扩展：私聊服务实现思路
1. Netty群聊扩展：私聊服务实现思路
    > 1. 群聊过程中，如果有用户想要进行一对一私聊，如何实现？
    >
    >    私聊功能，是群聊功能基础上的扩展，因此我们先考虑原本的结构是否可以实现
    >    
    >    如果想要进行一对一私聊，简单使用Netty提供的Channel组来存放所有客户端的Channel，无法满足我们的需要。
    >    
    >    我们需要追加一个能够特定存储每个客户端Channel的容器，因此可以添加一个HashMap来存放不同的Channel，并给每个用户一个用户名。
    >    
    >    如果需要进行私聊，可以规定私聊的信息格式，例如```私聊客户名：私聊信息```，然后我们在获取到信息时判断，如果有私聊客户名前缀，就截取出来，去Map中寻找对应用户的Channel，然后将信息转发给对应客户端Channel
    >    
### 12. Netty心跳检测机制
#### 1. Netty心跳检测实例
1. Netty心跳检测实例
   
    > 1. 因为客户端和服务器端之间的连接，需要进行维护，如果没有正常运行，需要进行相应的处理
    >
    > 2. 要求：Netty心跳检测实例，做到如下几个功能：1、服务器超过3秒没有读时，提示读空闲；2、服务器超过5秒没有进行写时，提示写空闲；3、服务器超过7秒没有读写时，提示读写空闲
    >
    > 3. 思路：
    >
    >    1. Netty已经提供给我们了一个Handler，用来进行空闲状态的心跳检测
    >
    >        IdleStateHandler，是Netty提供的，专门用来进行空闲状态的心跳检测的Handler
    >
    >        IdleStateHandler的构造器需要传入4个参数，这4个参数最为重要：
    >
    >        1、表示过了多久没有进行读操作，此时会发送一个心跳检测包检测连接状态
    >
    >        2、表示过了多久没有进行写操作，此时会发送一个心跳检测包检测连接状态
    >
    >        3、表示过了多久没有进行读写操作，此时会发送一个心跳检测包检测连接状态
    >
    >        4、表示上述参数的时间单位，可以使用TimeUtil提供的几个枚举类型作为实参
    >    
    >         * 问题：如果连接断开，handlerRemoved方法不是就会触发吗，为什么还需要心跳检测机制？因为有些特殊情况下，连接断开服务器无法感知，因此仅靠handlerRemoved方法无法应对这种特殊情况，因此需要心跳检测机制。
    >        
    >     2. 心跳检测机制检测到不可用连接后，我们可以自定义一个Handler添加到Pipeline中，用来处理这些不可用连接
    >    
    >        如果IdleStateHandler被触发，就会立刻传递给Pipeline中下一个Handler，并且触发下一个Handler中的userEventTiggered方法。
    >        
    >        因此我们可以在添加了IdleStateHandler后，必须立刻使用addLast方法，将处理不可用连接的Handler放入
    >        
    >         ```
    >        serverBootstrap.group(bossGroup, workerGroup)
    >                            .channel(NioServerSocketChannel.class)
    >                            .handler(new LoggingHandler(LogLevel.INFO)) // 打印一下bossGroup的日志
    >                            .childHandler(new ChannelInitializer<SocketChannel>() {
    >                                @Override
    >                                protected void initChannel(SocketChannel socketChannel) throws Exception {
    >        
    >                                    ChannelPipeline pipeline = socketChannel.pipeline();
    >        
    >                                    // 加入Netty提供的IdleStateHandler，做心跳检测
    >                                    pipeline.addLast(new IdleStateHandler(3, 5, 7, TimeUnit.SECONDS));
    >        
    >                                    // IdleStateHandler被触发，就会立刻传递给Pipeline中下一个Handler
    >                                    // 因此此时使用addLast方法，加入自定义的Handler，处理不可用连接
    >                                    pipeline.addLast(new NettyUselessConnectionSolveHandler);
    >        
    >                                }
    >                            });
    >         ```
    >        
#### 2. Netty心跳处理器
2. Netty心跳处理器
   
    >    1. 上述中提到，我们需要自定义一个Handler来处理不可用连接。如果IdleStateHandler被触发，就会立刻传递给Pipeline中下一个Handler，并且触发下一个Handler中的userEventTiggered方法。因此，我们需要让自定义的Handler重写userEventTiggered方法
    >
    >       ```
    >       public class NettyUselessConnectionSolveHandler extends ChannelInboundHandlerAdapter {
    >       
    >       
    >           /**
    >            * 当心跳检测的Handler被触发，该方法会被自动执行
    >            * ctx就是上下文
    >            * evt就是心跳检测机制检测到的事件
    >            * @param ctx
    >            * @param evt
    >            * @throws Exception
    >            */
    >           @Override
    >           public void userEventTriggered(ChannelHandlerContext ctx, Object evt) throws Exception {
    >       
    >               if (evt instanceof IdleStateEvent) {
    >                   // 确定传过来的事件，确实是IdleStateEvent。即心跳检测Handler传来的事件
    >       
    >                   // 强转一下事件
    >                   IdleStateEvent idleStateEvent = (IdleStateEvent) evt;
    >       
    >                   String eventMsg = null;
    >                   // 通过switch语句，对不同事件类型做处理
    >                   switch (idleStateEvent.state()) {
    >                       // 如果现在连接是读空闲
    >                       case READER_IDLE:
    >                           eventMsg = "读空闲状态";
    >                           break;
    >                       // 如果现在连接是写空闲
    >                       case WRITER_IDLE:
    >                           eventMsg = "写空闲状态";
    >                           break;
    >                       // 如果现在连接是读写空闲
    >                       case ALL_IDLE:
    >                           eventMsg = "读写空闲状态";
    >                           break;
    >                   }
    >                   
    >                   System.out.println(ctx.channel().remoteAddress() + "现在是" + eventMsg);
    >               }
    >       
    >           }
    >       
    >       }
    >       ```
    >
### 13. WebSocket编程
13. WebSocket编程
    > 1. Http协议是无状态协议，因此浏览器和服务器的请求响应一次，下回会重新建立连接。
    >
    > 2. 如果使用WebSocket编程，可以做到长连接的效果，如果浏览器和服务器交互频繁，可以节省大量的资源。
    >
    > 3. 要求：实现基于webSocket的长连接全双工的交互，改变Http协议的多次请求的约束，实现长连接，服务器可以发送信息给浏览器。浏览器和服务器可以互相感知，任何一方关闭了，对方都可以感知
    >
    > 4. 服务器端：
    >
    >    1. 程序主体：
    >
    >         ```java
    >          public class NettyWebSocketServer {
    >              public static void main(String[] args) throws Exception{
    >          
    >                  NioEventLoopGroup bossGroup = new NioEventLoopGroup();
    >               NioEventLoopGroup workerGroup = new NioEventLoopGroup();
    >          
    >                  ServerBootstrap serverBootstrap = new ServerBootstrap();
    >                  serverBootstrap.group(bossGroup, workerGroup)
    >                          .channel(NioServerSocketChannel.class)
    >                          .handler(new LoggingHandler(LogLevel.INFO))
    >                          .childHandler(new ChannelInitializer<SocketChannel>() {
    >                              @Override
    >                              protected void initChannel(SocketChannel socketChannel) throws Exception {
    >          
    >                                  ChannelPipeline pipeline = socketChannel.pipeline();
    >          
    >                                  // 因为基于Http协议，因此使用Http的编解码器
    >                                  pipeline.addLast(new HttpServerCodec());
    >          
    >                                  // 因为基于Http协议的数据传输，数据是以块来传输，因此还需要引入ChunkedWriteHandler
    >                                  pipeline.addLast(new ChunkedWriteHandler());
    >          
    >                                  // 因为基于Http协议的数据传输，数据传输时是分段的，即将大数据分段然后通过多次http请求发送分段数据
    >                                  // 因此需要将分段的数据聚合，HttpObjectAggregator就是聚合分段数据的处理器
    >                                  // 这也就是为什么，当浏览器发送大量数据时，就会发出多次http请求
    >                                  pipeline.addLast(new HttpObjectAggregator(8192));
    >          
    >                                  // WebSocketServerProtocolHandler参数为浏览器请求的应用程序上下文，如http://localhost:8600/hello中的/hello。
    >                                  // WebSocketServerProtocolHandler，是用来将Http协议升级为WebSocket协议，保持长连接
    >                                  pipeline.addLast(new WebSocketServerProtocolHandler("/hello"));
    >          
    >                                  // 自定义Handler，用来处理业务
    >                                  pipeline.addLast(null);
    >          
    >                              }
    >                          });
    >          
    >                  ChannelFuture sync = serverBootstrap.bind(8600).sync();
    >                  sync.channel().closeFuture().sync();
    >          
    >              }
    >              
    >          }
    >         ```
    >
    >     2. 具体的业务类：
    >
    >         ```Java
    >     public class NettyWebSocketHandler extends SimpleChannelInboundHandler<TextWebSocketFrame> {
    >          
    >              @Override
    >              protected void channelRead0(ChannelHandlerContext channelHandlerContext, TextWebSocketFrame textWebSocketFrame) throws Exception {
    >          
    >                  System.out.println("服务器端收到的消息为：" + textWebSocketFrame.text());
    >          
    >                  // 回写消息
    >                  channelHandlerContext.channel().writeAndFlush(new TextWebSocketFrame("服务器接收到消息，消息时间为：" + LocalDateTime.now()));
    >          
    >              }
    >          
    >              @Override
    >              public void handlerAdded(ChannelHandlerContext ctx) throws Exception {
    >          
    >          
    >                  System.out.println("handlerAdded被调用了：" + ctx.channel().id().asLongText());
    >          
    >              }
    >          
    >              @Override
    >              public void handlerRemoved(ChannelHandlerContext ctx) throws Exception {
    >          
    >                  System.out.println("handlerRemoved被调用了：" + ctx.channel().id().asLongText());
    >              }
    >          
    >              @Override
    >              public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
    >                  System.out.println("异常发生");
    >                  ctx.close();
    >              }
    >          
    >          }
    >         ```
    >
    > 5. 客户端：即浏览器网页编写
    >
    >     ```html
    >     <!DOCTYPE html>
    >     <html lang="en">
    >     <head>
    >         <meta charset="UTF-8">
    >         <title>Title</title>
    >     </head>
    >     <body>
    >         <form onsubmit="return false">
    >             <textarea name="message" style="height: 300px; width: 300px;">
    >     
    >             </textarea>
    >             <input type="button" value="发送消息" onclick="send(this.form.message.value)">
    >             <textarea id="responseText" style="height: 300px; width: 300px">
    >     
    >             </textarea>
    >             <input type="button" value="清空内容" onclick="document.getElementById('responseText').value=''">
    >         </form>
    >     </body>
    >     <script>
    >         var websocket;
    >         // 判断当前浏览器是否支持WebSocket
    >         if (window.WebSocket) {
    >             // WebSocket的url格式为ws://地址:端口/应用程序上下文
    >             websocket = new WebSocket("ws://localhost:8600/hello");
    >             // WebSocket编程中，浏览器端来接收服务器端信息的。ev参数为浏览器返回的信息
    >             websocket.onmessage = function (ev) {
    >                 let elementById = document.getElementById("responseText");
    >                 elementById.value = elementById.value + "\n" + ev.data;
    >             }
    >             // WebSocket编程中，浏览器开启和服务器的连接，之后可以在方法中写发送信息的逻辑。参数ev
    >             websocket.onopen = function (ev) {
    >                 let elementById = document.getElementById("responseText");
    >                 elementById.value = "连接已经开启";
    >             }
    >             // WebSocket编程中，浏览器监听是否连接还在继续。
    >             websocket.onclose = function (ev) {
    >                 let elementById = document.getElementById("responseText");
    >                 elementById.value = elementById.value + "\n" + "连接已经关闭！";
    >             }
    >     
    >         } else {
    >             alert("当前浏览器不支持WebSocket，请更换浏览器")
    >         }
    >     
    >         // 点击按钮，发送消息给服务器端。
    >         function send(message) {
    >             // 先判断WebSocket是否建立好
    >             if (window.websocket) {
    >                 // 判断websocket是否是开启状态
    >                 if (websocket.readyState == WebSocket.OPEN) {
    >                     websocket.send(message);
    >                 } else {
    >                     alert("连接还没有开启，稍等片刻");
    >                 }
    >             } else {
    >                 alert("连接还未建立好，请稍等片刻");
    >             }
    >         }
    >     </script>
    >     </html>
    >     ```
    >
    > 6. 为什么WebSocketServerProtocolHandler可以将Http协议，升级成WS协议。
    >
    >    WebSocketServerProtocolHandler，通过状态码101来切换协议。
    >
#### 1. WebSocketFrame
1. WebSocketFrame
    > 1. 对于WebSocket，它的数据是通过帧(frame)形式来传递
    > 
    > 2. Netty中，有专门的WebSocketFrame抽象类来处理WebSocket长连接。WebSocketFrame有六个具体的子类。
    > 
    >    * TextWebSocketFrame：表示一个文本帧(TextFrame)
    >    
### 14. Netty编码器机制
14. Netty编码器机制
    > 1. 编写的网络应用程序，数据在网络之间传输都是通过字节码进行的。因此，数据发送时要编码，数据接收时要解码
    >
    > 2. 编解码器(codec)包括两个组成部分：编码器(decoder)和解码器(encoder)。encoder目的是把数据编码为字节码，decoder目的是把字节码解码为真实数据。
    >
    > 3. Netty本身提供了一系列的codec
    >
    >     * Netty提供的编码器
    >     
    >         ```
    >         StringEncoder -- 对字符串进行编码 
    >         
    >         ObjectEncoder -- 对Java对象进行编码
    >         ```
    >         
    >     * Netty提供的解码器
    >     
    >         ```
    >         StringDecoder -- 对字符串解码
    >         
    >         ObjectDecoder -- 对Java对象解码
    >         ```
    >     
    > 4. Netty本身自带的ObjectEncoder和ObjectDecoder可以实现POJO对象或各种业务对象的编码和解码，底层仍然使用的是序列化和反序列化技术。Java本身的序列化技术效率不高，并且存在下列问题：
    > 
    >     1. 无法跨语言
    >     
    >     2. 序列化后的体积太大，是二进制编码的5倍还多
    >     
    >     3. 序列化性能太低
    >     
    > 5. 为了解决ObjectEncoder和ObjectDecoder上述的问题，引出了新的解决方案Google的Protobuf
    > 
#### 1. Protobuf
1. Protobuf
    > 1. Protobuf是Google发布的开源项目，是一种轻便高效的结构化存储格式，非常适合做数据存储或RPC数据交换格式。目前很多公司从Http+json向Tcp+Protobuf进行转型
    > 
    >     * RPC：远程过程调用
    >     
    > 2. Protobuf通过Message的方式来管理数据
    > 
    > 3. Protobuf有很好的跨平台、跨语言（客户端和服务器端可以是不同语言编写的）、高性能、高可靠等特性
    > 
    > 4. Protobuf的文件是.proto为后缀的，Idea中编写.proto文件，会自动提示是否下载对应Protobuf的插件，下载后可以做到Protobuf文件中语法高亮。.proto文件最终会被protoc.exe编译器编译为.java文件
    > 
    > 5. Protobuf数据传输流程：
    > 
    >     1. 先编写业务数据的.proto文件
    >     
    >     2. 通过protoc.exe将.proto文件编译成.java文件
    >     
    >     3. 通过Protobuf的编码器编码，然后传输
    >     
    >     4. 目标出，通过Protobuf的解码器解码，然后使用
    >     
    > 6. 业务数据的.proto文件编译成.java文件，该类其实是一个外部类嵌套了一个内部类。业务数据存储在内部类中。最终，发送的对象其实是内部类的对象
#### 2. Protobuf入门实例
2. Protobuf入门实例
    > 1. 要求：客户端发送一个Student的POJO对象给服务器（通过Protobuf编码），服务器接收到Student的POJO对象，并显示信息（通过Protobuf解码）
    >
    > 2. 实现步骤
    >
    >     1. 新建项目
    >     
    >     2. POM中引入Netty、Protobuf的坐标
    >     
    >     3. 编写具体业务数据的.proto文件
    >     
    >         ```
    >         syntax = "proto3"; // 表示ProtoBuf协议版本
    >         
    >         option java_outer_classname = "StudentPOJO"; // 最终会编译生成的外部类类名，同时也是.java文件的名称
    >         
    >         // protobuf 使用Message管理数据
    >         message Student { // 会在外部类中生成含具体业务数据的内部类，内部类名称就为Student。同时，是最终发送的POJO对象
    >           int32 id = 1; // int32是protobuf的类型，对应Java的int类型。最终Student内部类中，一个属性，类型为int，属性名为id，属性序号为1。
    >           string name = 2; // string对应Java中的String。最终会生成一个属性，类型为String，属性名为name，属性序号为2
    >         }
    >         ```
    >         
    >     4. 下载protoc.exe，为了方便，将.proto文件放入protoc.exe同级目录下
    >     
    >     5. 打开cmd，进入protoc.exe对应的路径，对.proto文件进行编译，编译命令为：```protoc.exe --java_out=. .proto文件文件名```。运行命令后，会在目录下生成对应的.java文件
    >     
    >     6. 在客户端的Handler中，定义发送对象的逻辑
    >     
    >         ```
    >         /**
    >              * SocketChannel就绪，触发该方法
    >              * @param ctx
    >              * @throws Exception
    >              */
    >             @Override
    >             public void channelActive(ChannelHandlerContext ctx) throws Exception {
    >                 // 发送StudentPOJO中内部类对象给服务器端
    >         
    >                 // 1. 创建对象，并给属性赋值
    >                 StudentPOJO.Student student = StudentPOJO.Student.newBuilder().setId(1).setName("张三").build();
    >                 // 2. 发送对象
    >                 ctx.channel().writeAndFlush(student);
    >         
    >             }
    >         ```
    >         
    >     7. 在客户端中，加入Protobuf的编码器，用来编码发送的对象为字节码
    >     
    >         ```
    >         // 设置相关参数
    >                     bootstrap.group(eventLoopGroup)
    >                             .channel(NioSocketChannel.class) // 设置客户端Channel的实现类
    >                             .handler(new ChannelInitializer<SocketChannel>() {
    >         
    >                                 // 向客户端的Pipeline添加Handler
    >                                 @Override
    >                                 protected void initChannel(SocketChannel socketChannel) throws Exception {
    >                                     ChannelPipeline pipeline = socketChannel.pipeline();
    >                                     // 加入编码器
    >                                     pipeline.addLast("encoder", new ProtobufEncoder());
    >                                     // 加入自己的处理器
    >                                     pipeline.addLast(new NettyTcpClientHandler());
    >                                 }
    >         
    >                             });
    >         ```
    >         
    >     8. 在服务器端中，加入Protobuf的解码器，用来将字节码解码成对象。
    >        
    >        ```
    >        // 使用链式编程设置参数
    >                    serverBootstrap.group(bossGroup, workerGroup)
    >                            .channel(NioServerSocketChannel.class) // 设置NioServerSocketChannel作为未来的服务器的ServerSocketChannel
    >                            .option(ChannelOption.SO_BACKLOG, 128) // 设置线程队列等待连接的个数
    >                            .childOption(ChannelOption.SO_KEEPALIVE, true) // 设置保持活动连接状态
    >                            .childHandler(new ChannelInitializer<SocketChannel>() {// ChannelInitializer是通道初始化对象
    >        
    >                                // 给Pipeline设置处理器
    >                                @Override
    >                                protected void initChannel(SocketChannel socketChannel) throws Exception {
    >                                    ChannelPipeline pipeline = socketChannel.pipeline();
    >                                    // 指定解码后生成的对象
    >                                    pipeline.addLast("decoder", new ProtobufDecoder(StudentPOJO.Student.getDefaultInstance()));
    >                                    pipeline.addLast(new NettyTcpServerHandler());
    >                                }
    >        
    >                            }); // 给workerGroup的某一个EventLoop对应的Pipeline设置处理器
    >        ```
    >        
    >    9. 在服务器端的Handler中，编写接收对象的逻辑
    >    
    >        ```
    >        /**
    >             * 用来读取客户端发送的消息，然后处理消息
    >             * Object 是客户端发送的数据，默认是Object，根据实际需要做类型转换
    >             * @param ctx
    >             * @param msg
    >             * @throws Exception
    >             */
    >            @Override
    >            public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
    >        
    >                // 读取从客户端发送来的对象，使用强转
    >                StudentPOJO.Student student = (StudentPOJO.Student) msg;
    >        
    >                System.out.println("客户端发送的数据为：id=" + student.getId() + ";name=" + student.getName());
    >        
    >            }
    >        ```
    >        
    >        * 如果该Handler不继承ChannelInboundHandlerAdapter，而是继承SimpleChannelInboundHandler<T>，我们就可以将对象类型直接写在泛型上。然后再channelRead方法中，msg参数就直接是指定的对象，不用强转了。
    >        
#### 3. Protobuf传输多种数据类型
3. Protobuf传输多种数据类型
    > 1. 按照上述的简单实例，客户端和服务器端之间，只能传输StudentPOJO.Student这一种对象。如何能够简单的传输多种对象呢？
    >
    > 2. Protobuf简单实例2：
    >
    >     1. 要求：客户端可以发送任意的数据类型(如Student的信息，Teacher的信息等等)到服务器端；服务器端可以接收到，并显示信息
    >     
    >     2. 实现步骤：
    >     
    >         1. 新建项目
    >     
    >         2. POM中引入Netty、Protobuf的坐标
    >     
    >         3. 编写具体业务数据的.proto文件
    >         
    >             因为protobuf可以使用Message来管理其他的Message，因此我们可以将多个Message的信息写在一个.proto文件中，然后用一个Message管理
    >             
    >              ```
    >             syntax = "proto3"; // 表示ProtoBuf协议版本
    >             option java_package = "com.yuu.protobufcase2"; // 指定生成的包结构。最终经过protoc.exe编译后，会生成对应的包，然后将类放入包中
    >             option optimize_for = SPEED;
    >             option java_outer_classname = "StudentPOJO"; // 最终会编译生成的外部类类名，同时也是.java文件的名称
    >             
    >             // protobuf 允许使用Message管理其他的Message
    >             
    >             message myMsg {
    >               // 1. 定义一个枚举类型
    >               enum DataType {
    >                 // proto3规范，要求enum编号必须从0开始
    >                 StudentType = 0;
    >                 WorkerType = 1;
    >               }
    >               // 2. 用DataType标识传入的是哪个枚举类型。我们定义了一个枚举类型，因此序号为1
    >               DataType data_type = 1;
    >               // 3. 表示每次枚举的类型，最多只能出现其中的一个，用来节省空间
    >               oneof dataBody {
    >                 Student student = 2;
    >                 Worker worker = 3;
    >               }
    >             }
    >             
    >             message Student { // 会在外部类中生成含具体业务数据的内部类，内部类名称就为Student。同时，是最终发送的POJO对象
    >               int32 id = 1; // int32是protobuf的类型，对应Java的int类型。最终Student内部类中，一个属性，类型为int，属性名为id，属性序号为1。
    >               string name = 2; // string对应Java中的String。最终会生成一个属性，类型为String，属性名为name，属性序号为2
    >             }
    >             
    >             message Worker {
    >               string name = 1;
    >               int32 age = 2;
    >             
    >             }
    >              ```
    >             
    >          3. 编译.proto文件
    >         
    >          4. 在客户端的Handler中，定义发送对象的逻辑
    >         
    >              ```
    >              /**
    >                   * SocketChannel就绪，触发该方法
    >                   * @param ctx
    >                   * @throws Exception
    >                   */
    >                  @Override
    >                  public void channelActive(ChannelHandlerContext ctx) throws Exception {
    >              
    >                      // 1. 创建对象，并给属性赋值
    >                      // 因为使用了一个Message管理其他Message，因此创建的是管理对象
    >                      // 根据管理对象的枚举类型，来创建并赋值对应的数据对象。例如这里发送一个Student的信息
    >                      StudentPOJO.MyMsg myMsg = StudentPOJO.MyMsg.newBuilder()
    >                              .setDataType(StudentPOJO.MyMsg.DataType.StudentType)
    >                              		.setStudent(StudentPOJO.Student.newBuilder().setId(2).setName("李四").build()).build();
    >              
    >                      // 2. 发送对象
    >                      ctx.writeAndFlush(myMsg);
    >              
    >                  }
    >              ```
    >              
    >          6. 客户端加入编码器
    >         
    >          7. 服务端加入解码器。注意，此时的解码对象为StudentPOJO.MyMsg
    >         
    >          8. 服务端的Handler定义接收逻辑
    >         
    >             先让Handler继承SimpleChannelInboundHandler<T>，泛型为StudentPOJO.MyMsg，然后书写逻辑
    >             
    >              ```
    >             @Override
    >                 protected void channelRead0(ChannelHandlerContext channelHandlerContext, StudentPOJO.MyMsg myMsg) throws Exception {
    >                     // 根据dataType显示不同的信息
    >                     StudentPOJO.MyMsg.DataType dataType = myMsg.getDataType();
    >                     if (dataType == StudentPOJO.MyMsg.DataType.StudentType) {
    >                         System.out.println("学生，客户端发送的数据为：id=" + myMsg.getStudent().getId() + ";name=" + myMsg.getStudent().getName());
    >                     } 
    >                     if (dataType == StudentPOJO.MyMsg.DataType.WorkerType) {
    >                         System.out.println("成员，客户端发送数据为：age=" + myMsg.getWorker().getAge() + ";name=" + myMsg.getWorker().getName());
    >                     }
    >                     
    >                 }
    >              ```
    >             
### 15. Netty的出站和入站
15. Netty的出站和入站
    > 1. 之前已经学习过Netty的核心组件，其中ChannelHandler充当了处理入站和出站数据的应用程序逻辑的容器
    > 
    > 2. 例如，如果实现了ChannelInboundHandler接口，就可以接受入站事件和数据，这些数据可以被业务逻辑处理，当要发送响应时，也可以在ChannelInboundHandler中冲刷数据。
    > 
    > 3. ChannelOutboundHandler原理和ChannelInboundHandler一样，不过是处理出站数据的。
    > 
    > 4. Netty提供的一系列编码器和解码器，它们也是Handler，都实现了ChannelInboundHandler接口或者ChannelOutboundHandler接口。
    > 
    > 5. 编码器继承了ChannelOutboundHandler，出站时，首先编码器器调用encode()方法对出站真实数据编码，然后将编码后的字节码数据传给下一个ChannelOutboundHandler；
    > 
    > 6. 解码器继承了ChannelInboundHandler，入站时，首先解码器调用decode()方法对入站字节码数据解码，然后将解码后的真实数据发送给下一个ChannelInboundHandler。
    > 
#### 1. ByteToMessageDecoder解码器
1. ByteToMessageDecoder解码器
    > 1. 关系继承图，参见视频
    > 
    > 2. 因为不知道远程发送数据，是否会一次性发送一个完整信息，tcp可能出现粘包拆包的问题，因此ByteToMessageDecoder会先对入站数据进行缓冲，直到它被处理好。
    > 
#### 2. MessageToByteEncoder<T>编码器
2. MessageToByteEncoder<T>编码器
    > 1. 用来对数据编码为字节码，泛型为需要进行编码的数据的类型。
    > 
### 16. Netty的Handler链调用机制
16. Netty的Handler链调用机制
    
    > 1. 要求：客户端发送long类型数据给服务器，服务器可以接受。客户端和服务端，都使用自定义的编码器和解码器来处理数据。
    >
    > 2. 服务器端
    >
    >    ```
    >    serverBootstrap.group(bossGroup, workerGroup)
    >                        .channel(NioServerSocketChannel.class)
    >                        .childHandler(new ChannelInitializer<SocketChannel>() {
    >                            @Override
    >                            protected void initChannel(SocketChannel socketChannel) throws Exception {
    >                                ChannelPipeline pipeline = socketChannel.pipeline();
    >                                // 加入自定义入站的解码器 MyByteToLongDecoder
    >                                pipeline.addLast(new MyByteToLongDecoder());
    >                                // 加入逻辑处理Handler
    >                                pipeline.addLast(new MyServerHandler());
    >                            }
    >                        });
    >    ```
    >
    >    ```
    >    public class MyByteToLongDecoder extends ByteToMessageDecoder {
    >    
    >        /**
    >         * ctx为上下文，byteBuf为入站字节码放入的缓冲区，list为解码后的数据放入的集合
    >         * @param channelHandlerContext
    >         * @param byteBuf
    >         * @param list
    >         * @throws Exception
    >         */
    >        @Override
    >        protected void decode(ChannelHandlerContext channelHandlerContext, ByteBuf byteBuf, List<Object> list) throws Exception {
    >    
    >            // 客户端传入的long类型数据，因此先保证byteBuf中至少有8个字节
    >            if (byteBuf.readableBytes() >= 8) {
    >                // 将byteBuf中所有字节，读取为long类型数据，放入list
    >                list.add(byteBuf.readLong());
    >            }
    >    
    >        }
    >    }
    >    ```
    >
    >    * 解码器的decode方法，会根据接受的数据，被调用多次，直到没有新的原色被添加到list中或byteBuf中没有更多可读字节为止。如果list不为空，则会将list的内容，传递给下一个ChannelInboundHandler，该过程也会持续多次，直到list没有新内容传递过来。
    >
    >    ```
    >    public class MyServerHandler extends SimpleChannelInboundHandler<Long> {
    >        @Override
    >        protected void channelRead0(ChannelHandlerContext channelHandlerContext, Long aLong) throws Exception {
    >    
    >            // 打印数据
    >            System.out.println("从客户端读取到的数据为：" + aLong);
    >    
    >        }
    >    
    >        @Override
    >        public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
    >            ctx.close();
    >        }
    >    }
    >    ```
    >
    > 3. 客户端
    >
    >    ```
    >    bootstrap.group(eventLoopGroup)
    >                        .channel(NioSocketChannel.class)
    >                        .handler(new ChannelInitializer<SocketChannel>() {
    >                            @Override
    >                            protected void initChannel(SocketChannel socketChannel) throws Exception {
    >    
    >                                ChannelPipeline pipeline = socketChannel.pipeline();
    >                                // 加入出站的编码器 MyClient
    >                                pipeline.addLast(new MyLongByteEncoder());
    >                                // 加入自定义Handler
    >                                pipeline.addLast(new MyClientHandler());
    >                            }
    >                        });		
    >    ```
    >
    >    ```
    >    public class MyLongByteEncoder extends MessageToByteEncoder<Long> {
    >        @Override
    >        protected void encode(ChannelHandlerContext channelHandlerContext, Long aLong, ByteBuf byteBuf) throws Exception {
    >            // 将long类型数据，写入byteBuf中
    >            byteBuf.writeLong(aLong);
    >        }
    >    }
    >    ```
    >
    >    ```
    >    public class MyClientHandler extends SimpleChannelInboundHandler<Long> {
    >        @Override
    >        protected void channelRead0(ChannelHandlerContext channelHandlerContext, Long aLong) throws Exception {
    >    
    >        }
    >    
    >        /**
    >         * 当通道准备好，发送数据给服务器端
    >         * @param ctx
    >         * @throws Exception
    >         */
    >        @Override
    >        public void channelActive(ChannelHandlerContext ctx) throws Exception {
    >            ctx.writeAndFlush(123456L);
    >        }
    >    }
    >    ```
    >    
#### 1. Handler链调用原理剖析
1. Handler链调用原理剖析
    > 1. 上述使用自定义的编码器和解码器发送long类型数据时，发现编码器和解码器都被调用了。但是当发送和接收String数据时，自定义的编码器和解码器没有调用。为什么呢？
    > 2. 因为自定义的编码器和解码器继承的两个父类中，有一个方法，专门用来判断要编码和解码的数据是否是指定的数据，不是就不调用，继续转送给其他Handler。
    > 
### 17. Netty常用的其他编码器和解码器
#### 1. RepalyingDecoder<S>解码器
1. RepalyingDecoder<S>解码器
    > 1. public abstract ReplayingDecoder<S> extends ByteToMessageDecoder
    > 
    > 2. ReplayingDecoder扩展了ByteToMessageDecoder，如果我们让自定义的解码器Handler继承该类，则在重写的decode方法中不必调用readbleBytes方法判断数据了，因为ReplayingDecoder底层会自动做判断。
    > 
    > 3. ReplayingDecoder<S>的泛型S指定的是用户状态管理的类型，如果泛型S使用Void，则代表不需要进行用户状态管理。
    > 
    > 4. ReplayingDecoder的局限性
    > 
    >     * 并不是所有的ByteBuf都能被支持，如果调用了不被支持的方法，则会直接抛出UnsupportedOperationException异常
    >     
    >     * 在某些情况下，ReplayingDecoder的处理速度慢于ByteToMessageDecoder，例如网络缓慢、消息格式复杂时，消息被拆成多个碎片，速度变慢
    >     
#### 2. 其他更多的解码器
2. 其他更多的解码器：都是ByteToMessageDecoder的子类
    > 1. LineBasedFrameDecoder
    > 
    >     1. 在Netty内部有使用，它使用换行符(```\n或\r\n```)作为分隔符解析数据。
    >     
    > 2. DelimiterBasedFrameDecoder
    > 
    >     1. 使用自定义的特殊字符作为消息的分隔符
    >     
    > 3. HttpObjectDecoder
    > 
    >     1. Http数据的解码器
    >     
    > 4. LengthFiledBasedFrameDecoder
    > 
    >     1. 通过指定长度来标识消息，这样就可以自动处理黏包和半包消息
    >     
#### 3. 其他更多的编码器
3. 其他更多的编码器：都是MessageToByteDecoder的子类
   
    > 1. 不再一一列举
    > 
### 18. TCP粘包和拆包
18. TCP粘包和拆包
    > 1. TCP介绍
    >
    >    TCP是面向连接，面向流的，它提供了高可靠的服务。
    >    
    >    收发两端，必须有一一成对的Socket，因此发送端为了将多个数据包更有效的发送给接收端，使用了优化的算法(Nagle算法)：将多次间隔较小并且数据量小的数据包，合并成一个大的数据块，然后进行封包发送。
    >    
    >    Nagle算法虽然提高了发送效率，但是接收端就难以分辨出完成的数据包了，**因为面向流的通信没有消息保护边界**
    >    
    > 2. 因为TCP没有消息保护边界，因此接收端必须处理消息边界问题，也就是我们所说的粘包、拆包问题。
    >
    > 3. 粘包和拆包介绍：以发送端发送两个数据包D1和D2为例子
    >
    >     1. 粘包
    >     
    >        接收端一次接收到两个数据包，D1和D2粘合在一起。这就叫做粘包
    >        
    >     2. 拆包
    >     
    >        接收端两次接收到两个数据包，第一次接收到D1全部和D2部分内容，第二次接收到D2剩余内容；或者第一次接收到D1部分内容，第二次接收到D1剩余内容和D2全部内容。这就叫做拆包
    >     
#### 1. 粘包和拆包问题解决
1. 粘包和拆包问题解决
    > 1. 解决思路
    >
    >    TCP粘包和拆包的解决，就是解决每次接收端读取数据长度的问题，该问题解决了就解决了TCP粘包和拆包问题
    >
    >    如果我们发送数据时，可以将数据的长度等信息先发送过去，然后按照数据长度来读取数据，即可解决问题
    >    
    >    通过自定义协议+编解码器，解决粘包和拆包问题
    >    
#### 2. Netty解决粘包和拆包案例
2. Netty解决粘包和拆包案例：参见D:/netty_code_home/netty_code/com/yuu/nettysolvetcpproblem
    > 1. 要求：服务器端发送5个Message对象，每次发送1个Message；客户端端每次接受一个Message，分5次进行解码，每读取到一个Message
    >
    > 2. 实现：
    >
    >     1. 服务器端
    >        1. 自定义协议类
    >        2. 自定义Handler
    >        3. 自定义编码器
    >        4. 定义Server主程序
    >     2. 客户端
    >        1. 将协议类复制到客户端项目中
    >        2. 自定义解码器
    >        3. 自定义Handler
    >        4. 定义Client主程序