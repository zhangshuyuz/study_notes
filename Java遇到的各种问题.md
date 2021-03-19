#  1. Java8新特性：默认方法带来的问题

1.  Java8新特性：默认方法带来的问题：
   * 问题描述：我们知道Java语言中一个类只能继承一个父类，但是一个类可以实现多个接口。随着默认方法在Java8中的引入，有可能出现一个类继承了多个签名一样的方法。这种情况下，类会选择使用哪一个函数呢？
   * 解决方案：为解决这种多继承关系，Java8提供了下面三条规则：
     1.  类中的方法优先级最高，类或父类中声明的方法的优先级高于任何声明为默认方法的优先级。
     2.  如果第一条无法判断，那么子接口的优先级更高：方法签名相同时，优先选择拥有最具体实现的默认方法的接口， 即如果B继承了A，那么B就比A更加具体。
     3.  最后，如果还是无法判断，继承了多个接口的类必须通过显式覆盖和调用期望的方法， 显式地选择使用哪一个默认方法的实现。

# 2. 枚举类中的values()方法
2. 枚举类中的values()方法

    * 介绍：
    * 
       在我们编写自定义的enum时，其中是不含values方法的，再编译java文件时，java编译器会自动帮助我们生成这个方法
       
       该方法会返回枚举类中，枚举对象数组
       
# 3. Java中的对象克隆
3. Java中的对象克隆
    > 1. Java如何实现对象的克隆。
    >
    >     1. 实现Cloneable接口，并重写继承自Object类中的clone()方法
    >     
    >     2. 通过类的序列化和反序列化进行克隆，实现真正的深度克隆
    >     
    >         * 也就是通过流的方式进行克隆。使用基于内存的流ByteArrayInputStream、ByteArrayOutputStream、ObjectInputStream、ObjectOutputStream来对对象进行克隆，ByteArrayInputStream、ByteArrayOutputStream流有个特点：只要垃圾回收器清理了对象，就会自动释放流的资源，不同于其他的流
    >     
    >         * 如果我们定义了一个工具类，内置一个方法用来对类进行序列化的方式进行克隆
    >         
    >             ```
    >             public class Multi {
    >             
    >                 private Multi() {
    >                 }
    >             
    >                 @SuppressWarnings("unchecked")
    >                 public static <T extends Serializable> T clone(T obj) throws IOException, ClassNotFoundException {
    >                 
    >                     ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
    >                     ObjectOutputStream objectOutputStream = new ObjectOutputStream(byteArrayOutputStream);
    >                     objectOutputStream.writeObject(obj);
    >             
    >                     ByteArrayInputStream byteArrayInputStream = new ByteArrayInputStream(byteArrayOutputStream.toByteArray());
    >                     ObjectInputStream objectInputStream = new ObjectInputStream(byteArrayInputStream);
    >                     return (T)objectInputStream.readObject();
    >                     
    >                 }
    >             
    >             }
    >             ```
    >         
    >         * 好处：序列化和反序列化方式不仅是深度克隆，更重要的是可以根据泛型上限，来检测要克隆的对象是否支持序列化，这个检查是编译器执行的，不会在运行时抛出异常。因此该方案优于重写clone()方法的方案。
    >     
    > 2. Java中深拷贝和浅拷贝的区别
    > 
    >     * 浅拷贝：拷贝对象的引用地址，两个对象指向的是同一个对象内存地址，因此如果任意一个对象修改了内存中的值，另一个的也会改变
    >     
    >     * 深拷贝：开辟新的内存，将对象和值都复制过来。但是深拷贝无法复制函数类型
    > 3. BeanUtils是深拷贝，还是浅拷贝?
    > 
    >    浅拷贝。如果都是单一的属性，那么不涉及到深拷贝的问题，适合用BeanUtils。
    >    
    > 4. 有子对象就一定不能用BeanUtils么
    > 
    >    并不绝对，这个要区分考虑：
    >    
    >    1、子对象还要改动。
    >    
    >    2、子对象不怎么改动。
    >    
    >    虽然有子对象，但是子对象并不怎么改动，那么用BeanUtils也是没问题的。
    >    
# 4. 两个小问题
4. 两个小问题
    > 1. Java中，float f = 3.14是否正确？
    >
    >    不正确，因为3.14是double类型，向下转型要强转或者使用F
    >    
    > 2. Java中，```&```和```&&```的区别
    >
    >    ```&```是按位与、逻辑与的意思；```&&```是短路运算符，表示与
    >    
# 5. IDEA中查看类图的快捷键
5. IDEA中查看类图的快捷键
   
    > 1. Ctrl + H
    > 
# 6. Math.round()方法
6. Math.round()方法
   
    > 1. Math.round()方法用来对一个数进行四舍五入。它的原理为：先将数+0.5，然后进行强制转换，转为int
    > 
# 7. +=、-=的特性
7. +=、-=的特性
   
    > 1. +=、-=，它们会自动进行数据类型转换
    > 
# 8. Java的双亲委派机制和沙箱安全机制
8. Java的双亲委派机制和沙箱安全机制
    > 1. 双亲委派机制
    > 
    >     1. 介绍
    > 
    >        某个类加载器需要加载某个.class文件时，它首先把这个任务委托给他的上级类加载器，递归这个操作，如果上级的类加载器没有加载，自己才会去加载这个类。
    >       
    >     2. 作用
    > 
    >        1、防止重复加载同一个.class。通过委托去向上面问一问，加载过了，就不用再加载一遍。保证数据安全。
    >       
    >        2、保证核心.class不能被篡改。通过委托方式，不会去篡改核心.class，即使篡改也不会去加载，即使加载也不会是同一个.class对象了。不同的加载器加载同一个.class也不是同一个Class对象。这样保证了Class执行安全。
    >     
    > 2. 沙箱安全机制
    > 
    >     1. 介绍
    >     
    >        Java为了保护自己原生的一些类不被篡改而提出的机制。 
    >        
    >        例如，如果我们自己定义了java.lang包，并且在里面自定义了String类，我们使用自定义的java.lang.String会立刻报错，这就是沙箱安全机制
    >        
# 9. HashSet的底层原理
9. HashSet的底层原理
   
    > 1. HashSet底层是由HashMap来实现的，HashSet的值会放在HashMap的Key位置上，因此HashSet不允许有重复的值。 
    > 
# 10. 缓冲和缓存
10. 缓冲和缓存
    > 1. 缓冲
    > 
    >    缓冲，意思是减少冲击，减少的是写的冲击。目的是为了避免上下处理IO速度不同，导致的阻塞
    >    
    > 2. 缓存
    > 
    >    缓存，意思是有个地方存储数据。缓存会将经常用的数据放入缓存，用来减少读操作对数据库的压力。
    >    
# 11. IDEA切换选中单词的大小写快捷键
11. IDEA切换选中单词的大小写快捷键
    
    > <u>*Ctrl+Shift+u *</u>
    > 
# 12. 故障解决步骤
12. 故障解决步骤
    > 1. 步骤
    > 
    >     1. 定位问题
    >     
    >     2. 问题原因
    >     
    >     3. 解决问题
    >     
    >     4. 优化方案(让该故障下次不再发生)
    >     
# 13. 常见的异常和导致的原因整理
13. 常见的异常和导致的原因整理
    > 1. 常见异常和导致原因整理
    > 
    >     * 空指针异常
    >     
    >     * ```java.util.ConcurrentModificationException```，并发写异常
    >     
    >     * ```java.lang.IllegalStateExeception```，阻塞队列为满时使用add(E e)方法抛出的异常
    >     
    >     * ```java.util.NoSuchElementExeception```，阻塞队列为空时使用remove()方法抛出的异常
    >     
# 14. 学习新技术的步骤
14. 学习新技术的步骤
    > 1. 先理论，后实践，最后总结。
    > 
    > 2. 对新知识，做到：为什么用，是什么理论，怎么用
    > 
# 15. Java故障排查步骤
15. Java故障排查步骤
    > 1. 在IDEA中，输入alt+F12进入项目所在的cmd窗口
    > 
    > 2. 输入```jps -l```获取项目所有的进程的进程信息，找到异常所在的进程
    > 
    > 3. 输入```jstack 异常进程的进程号```，获取进程的异常信息