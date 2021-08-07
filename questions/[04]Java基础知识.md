- [\# String, StringBuilder, StringBuffer 的区别？](#-string-stringbuilder-stringbuffer-的区别)
- [\# `String a = "hello2"; String b = "hello" + 2; `，a 和 b 是否相等，为什么？](#-string-a--hello2-string-b--hello--2-a-和-b-是否相等为什么)
- [\# `String a = "hello2"; String b = "hello"; String c = b + 2;`，a 和 c 是否相等，为什么？](#-string-a--hello2-string-b--hello-string-c--b--2a-和-c-是否相等为什么)
- [\# `String a = "hello2"; final String b = "hello"; String c = b + 2;`， a 和 c 是否相等，为什么？](#-string-a--hello2-final-string-b--hello-string-c--b--2-a-和-c-是否相等为什么)
- [\# `String a = "abc"；`这个过程会创建什么，放在哪里?](#-string-a--abc这个过程会创建什么放在哪里)
- [\# String 为什么不能被继承？](#-string-为什么不能被继承)
- [\# 对象的深浅复制](#-对象的深浅复制)
- [\# Java 枚举类比较是用==还是equals？](#-java-枚举类比较是用还是equals)
- [\# volatile关键词的作用，为什么能实现内存可见性和禁止重排？](#-volatile关键词的作用为什么能实现内存可见性和禁止重排)
- [\# synchronized 的底层实现原理是什么？](#-synchronized-的底层实现原理是什么)
- [\# synchronized 锁范围？](#-synchronized-锁范围)
- [\# synchronized 是否可重入？](#-synchronized-是否可重入)
- [\# synchronized 和 Lock 的差异是什么？](#-synchronized-和-lock-的差异是什么)
- [\# Java有哪些锁?](#-java有哪些锁)
- [\# Java线程安全容器？](#-java线程安全容器)
- [\# 锁膨胀是怎么回事?](#-锁膨胀是怎么回事)
- [\# 什么是AQS？](#-什么是aqs)
- [\# ReadWriteLock读写之间互斥吗？](#-readwritelock读写之间互斥吗)
- [\# private修饰的方法可以通过反射访问，那么private的意义是什么？](#-private修饰的方法可以通过反射访问那么private的意义是什么)
- [\# Java类初始化顺序是什么？](#-java类初始化顺序是什么)
- [\# 类加载过程](#-类加载过程)
- [\# 双亲委派模型是什么？它的作用是什么？](#-双亲委派模型是什么它的作用是什么)
- [\# 两个自定义加载器加载同一个类，是否会加载两次？](#-两个自定义加载器加载同一个类是否会加载两次)
- [\# 一个Java文件有3个类，编译后有几个class文件？](#-一个java文件有3个类编译后有几个class文件)
- [\# 局部变量使用前需要显式地赋值，否则编译通过不了，为什么这么设计？](#-局部变量使用前需要显式地赋值否则编译通过不了为什么这么设计)
- [\# `System.out.println()` 这行代码，每个符号是什么？](#-systemoutprintln-这行代码每个符号是什么)
- [\# 强引用、软引用、弱引用、虚引用](#-强引用软引用弱引用虚引用)
- [\# equals 与 ==](#-equals-与-)
- [\# try-catch 结构时候，如果代码抛出 `throw new XException` , 会被以下那句捕获？](#-try-catch-结构时候如果代码抛出-throw-new-xexception--会被以下那句捕获)
- [\# try-catch-final 结构，什么情况下 final 不被执行？](#-try-catch-final-结构什么情况下-final-不被执行)
- [\# servlet是否线程安全，如何改造保证线程安全？](#-servlet是否线程安全如何改造保证线程安全)
- [\# Semaphore拿到执行权的线程之间是否互斥？](#-semaphore拿到执行权的线程之间是否互斥)
- [\# CountDownLatch 和 CyclicBarrier 的源码实现和区别？](#-countdownlatch-和-cyclicbarrier-的源码实现和区别)

## \# String, StringBuilder, StringBuffer 的区别？
- String 是不可变的对象，默认都是以常量形式保存在了常量池中，且由final修饰，因此在每次对 String 类型进行改变的时候其实都等同于生成了一个新的 String 对象，然后将指针指向新的 String 对象，所以经常改变内容的字符串每次生成对象会对系统性能产生影响。String是线程安全的，但是效率低下，适合少量字符串操作的情况。
- StringBuilder 继承自 AbstractStringBuilder ，本质是一个可变的字符序列，对于大量字符串操作效率会比较高，但是不保证线程安全。
- StringBuffer 也继承自 AbstractStringBuilder，内部使用 synchronize 关键字修饰，因此可以保证线程安全。

## \# `String a = "hello2"; String b = "hello" + 2; `，a 和 b 是否相等，为什么？
相等，因为 "a" + "b" 在编译期间会被优化成 "ab”，所以在运行期间a和b是常量池中的同一个对象。

## \# `String a = "hello2"; String b = "hello"; String c = b + 2;`，a 和 c 是否相等，为什么？
不相等，因为引用符号的存在，b + 2不会在编译期间被优化，不会把 b + 2 当做字面常量来处理，这种生成的对象是保存在堆上的，而 a 的对象是在常量池中，故a和c指向的并不是同一个对象。

## \# `String a = "hello2"; final String b = "hello"; String c = b + 2;`， a 和 c 是否相等，为什么？
相等，因为对于被 final 修饰的变量，会在常量池中保存一个副本，也就是说不会通过引用而进行访问，对 final 变量的访问在编译期间都会直接被替代为真实的值，那么String c = b + 2 就等同于 String c = “hello” + 2。

## \# `String a = "abc"；`这个过程会创建什么，放在哪里?
首先会查找StringTable，如果找到则直接返回引用给a，如果没有则会在堆中会创建一个 "abc" 实例，全局StringTable中存放着 "abc" 的引用值。

## \# String 为什么不能被继承？
因为String类有final修饰符，而final修饰的类是不能被继承的，并且String类实际是一个被final关键字修饰的char[]数组。之所以这么设计，一方面是为了安全性的考虑，不可变保证线程安全，保证字符串对象不被随便篡改，一方面是为了性能上的考虑，只有当字符串是不可变的，字符串池才有可能实现，可以在字符串创建时就把 hashCode 缓存。

## \# 对象的深浅复制
深复制是把对象完整的复制一遍，而浅复制只是复制对象的引用。

## \# Java 枚举类比较是用==还是equals？
都一样，因为在Enum类里面，已经重写了equals方法，而方法里面比较就是直接使用 == 来比较对象的。

## \# volatile关键词的作用，为什么能实现内存可见性和禁止重排？
volatile主要用来保证变量的内存可见性和禁止指令重排序。
- 使用 volatile 修饰共享变量后，每个线程要操作变量时会从主内存中将变量拷贝到本地内存作为副本，当线程操作变量副本并写回主内存后，会通过 CPU 总线嗅探机制告知其他线程该变量副本已经失效，需要重新从主内存中读取。
- 使用 volatile 修饰变量时，根据 volatile 重排序规则表，Java 编译器在生成字节码时，会在指令序列中插入内存屏障指令来禁止特定类型的处理器重排序。

## \# synchronized 的底层实现原理是什么？
synchronized的底层原理是跟monitor有关，也就是视图器锁，每个对象都有一个关联的monitor，当synchronize获得monitor对象的所有权后会进行两个指令：加锁指令monitorenter跟减锁指令monitorexit。
monitor里面有个计数器，初始值是从0开始的。如果一个线程想要获取monitor的所有权，就看看它的计数器是不是0，如果是0的话，那么就说明没人获取锁，那么它就可以获取锁了，然后将计数器+1，也就是执行monitorenter加锁指令；monitorexit减锁指令是跟在程序执行结束和异常里的，如果不是0的话，就会陷入一个堵塞等待的过程，直到为0等待结束。

## \# synchronized 锁范围？
- 普通同步方法，锁是当前实例对象
- 静态同步方法，锁是当前类的class对象
- 同步方法块，锁是括号里面的对象

## \# synchronized 是否可重入？
synchronized 是可重入锁！

## \# synchronized 和 Lock 的差异是什么？
- Lock是一个接口，而synchronized是java的一个关键字；
- synchronized在发生异常时会自动释放占有的锁，因此不会出现死锁；而lock发生异常时，不会主动释放占有的锁，必须手动来释放锁，可能引起死锁的发生；
- Lock等待锁过程中可以用interrupt来中断等待，而synchronized只能等待锁的释放，不能响应中断；
- Lock可以通过tryLock来知道有没有获取锁，而synchronized不能；

## \# Java有哪些锁?
- 公平锁/非公平锁
- 可重入锁/不可重入锁
- 独享锁/共享锁
- 乐观锁/悲观锁
- 偏向锁/轻量级锁/重量级锁
- 自旋锁

## \# Java线程安全容器？
- Vector
- HashTable
- ConcurrentHashMap
- CopyOnWriteArrayList
- CopyOnWriteArraySet
- ConcurrentLinkedQueue
- ConcurrentSkipListMap
- ConcurrentSkipListSet

## \# 锁膨胀是怎么回事?
当一个线程第一次获取锁后再去拿锁就是偏向锁，如果有别的线程和当前线程交替执行就膨胀为轻量级锁，如果发生竞争就会膨胀为重量级锁。

## \# 什么是AQS？
AbstractQueuedSynchronizer简称AQS，是一个用于构建锁和同步容器的框架。AQS使用一个FIFO的队列表示排队等待锁的线程，它维护一个status的变量，每个节点维护一个waitstatus的变量，当线程获取到锁的时候，队列的status置为1，此线程执行完了，那么它的waitstatus为-1；队列头部的线程执行完毕之后，它会调用它的后继的线程。

## \# ReadWriteLock读写之间互斥吗？
读读共享，读写互斥，写写互斥。

## \# private修饰的方法可以通过反射访问，那么private的意义是什么？
private的存在并不是为了实现 Java 程序的完全安全，而只是为了 Java 的正常开发过程中的一种约束，也是符合 OOP 的封装，实现内部的部分不可见。

## \# Java类初始化顺序是什么？
静态变量、静态代码块 -> 变量、初始化块 -> 构造器
父类 -> 子类
1. 父类静态代码块和静态成员变量并列优先级；
2. 子类静态代码块和静态成员变量并列优先级；
3. 实例化对象（new）之前的代码；
4. 父类构造代码块和普通成员变量并列优先级；
5. 父类构造方法；
6. 子类构造代码块和普通成员变量并列优先级；
7. 子类构造方法；
8. 实例化对象（new）之后的代码。

## \# 类加载过程
1. 加载：类加载阶段就是由类加载器负责根据一个类的全限定名来读取此类的二进制字节流到JVM内部，并存储在运行时内存区的方法区，然后将其转换为一个与目标类型对应的java.lang.Class对象实例，这个Class对象在日后就会作为方法区中该类的各种数据的访问入口；
2. 验证：验证类数据信息是否符合JVM规范，是否是一个有效的字节码文件，验证内容涵盖了类数据信息的格式验证、语义分析、操作验证等；
3. 准备：为类中的所有静态变量分配内存空间，并为其设置一个初始值（由于还没有产生对象，实例变量不在此操作范围内）；
4. 解析：将常量池中的符号引用转为直接引用（得到类或者字段、方法在内存中的指针或者偏移量，以便直接调用该方法），这个可以在初始化之后再执行。
5. 初始化：将一个类中所有被static关键字标识的代码统一执行一遍，如果执行的是静态变量，那么就会使用用户指定的值覆盖之前在准备阶段设置的初始值；如果执行的是static代码块，那么在初始化阶段，JVM就会执行static代码块中定义的所有操作。

## \# 双亲委派模型是什么？它的作用是什么？
如果一个类加载器收到了类加载的请求，它首先不会自己去尝试加载这个类，而是把这个请求委托给父类加载器去完成，每一个层次的类加载器都是如此，因此所有的加载请求最终都应该传送到顶层的启动类加载器中，只有当父类加载器反馈自己无法完成这个加载请求（它的搜索范围中没有找到所需要加载的类）时，子加载器才会尝试自己去加载。
它的作用是为了能够有效确保一个类的全局唯一性，当程序中出现多个限定名相同的类时，类加载器在执行加载时，始终只会加载其中的某一个类。

## \# 两个自定义加载器加载同一个类，是否会加载两次？
应该是可以的。(待求证)

## \# 一个Java文件有3个类，编译后有几个class文件？
如果一个java文件中有多个类,除去内部类,剩下的每个普通类都会生成一个class文件。

## \# 局部变量使用前需要显式地赋值，否则编译通过不了，为什么这么设计？
对于局部变量而言，其赋值和取值访问顺序是确定的。这样设计是一种约束，尽最大程度减少使用者犯错的可能。假使局部变量可以使用默认值，可能总会无意间忘记赋值，进而导致不可预期的情况出现。

## \# `System.out.println()` 这行代码，每个符号是什么？
System 是系统类，out 是标准输出对象，println() 是一个方法。

## \# 强引用、软引用、弱引用、虚引用
- 强引用：把一个对象赋给一个引用变量，这个引用变量就是一个强引用；
- 软引用：软引用用来描述一些还有用，但并非必需的对象。对于软引用关联着的对象，在系统将要发生内存溢出异常之前，将会把这些对象列进回收范围之中并进行第二次回收。如果这次回收还是没有足够的内存，才会抛出内存溢出异常；
- 弱引用：弱引用也是用来描述非必需对象的，但是它的强度比软引用更弱一些，被弱引用关联的对象只能生存到下一次垃圾收集发生之前。当垃圾收集器工作时，无论当前内存是否足够，都会回收掉只被弱引用关联的对象；
- 虚引用：设置虚引用的唯一目的，就是在这个对象被回收器回收的时候收到一个系统通知或者后续添加进一步的处理；

## \# equals 与 ==
- ==是判断两个变量或实例是不是指向同一个内存空间，equals是判断两个变量或实例所指向的内存空间的值是不是相同；
- ==是指对内存地址进行比较 ， equals()是对内容进行比较；
- ==指引用是否相同， equals()指的是值是否相同；

## \# try-catch 结构时候，如果代码抛出 `throw new XException` , 会被以下那句捕获？
```java
catch YException
catch Exception
catch XException
```
应该会被第二句捕获。

## \# try-catch-final 结构，什么情况下 final 不被执行？
在try块或者catch块中调用了退出虚拟机的方法（即`System.exit(1);`）

## \# servlet是否线程安全，如何改造保证线程安全？
不是线程安全的。当Tomcat接收到Client的HTTP请求时，Tomcat从线程池中取出一个线程，之后找到该请求对应的Servlet对象并进行初始化，之后调用service()方法。要注意的是每一个Servlet对象再Tomcat容器中只有一个实例对象，即是单例模式。如果多个HTTP请求请求的是同一个Servlet，那么着两个HTTP请求对应的线程将并发调用Servlet的service()方法。如果Servlet1中定义了实例变量或静态变量，那么可能会发生线程安全问题。
实现线程安全的方法：尽可能不创建成员变量，因为成员变量会被多个线程共享；同步对共享数据的操作；实现SingleThreadModel 接口。

## \# Semaphore拿到执行权的线程之间是否互斥？
不是互斥的，Semaphore 只是限制可以同时访问的线程的数量。

## \# CountDownLatch 和 CyclicBarrier 的源码实现和区别？
- CountdownLatch 适用于所有线程通过某一点后通知方法，而CyclicBarrier则适合让所有线程在同一点同时执行。
- CountdownLatch 利用继承AQS的共享锁来进行线程的通知，利用CAS来进行，而 CyclicBarrier 则利用 ReentrantLock 的Condition来阻塞和通知线程。