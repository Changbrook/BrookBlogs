#### 建议的参考链接
https://www.ibm.com/developerworks/cn/java/j-spring-boot-basics-perry/index.html

#### sleep() 和 wait() 有什么区别? 
1、sleep就是正在执行的线程主动让出cpu，cpu去执行其他线程，在sleep指定的时间过后，cpu才会回到这个线程上继续往下执行 
1.2、如果当前线程进入了同步锁，sleep方法并不会释放锁，即使当前线程使用sleep方法让出了cpu，但其他被同步锁挡住了的线程也无法得到执行。 
2、wait是指在一个已经进入了同步锁的线程内，让自己暂时让出同步锁，以便其他正在等待此锁的线程可以得到同步锁并运行 
2.2、只有其他线程调用了notify方法，调用wait方法的线程就会解除wait状态和程序可以再次得到锁后继续向下运行。 
【注意】notify并不释放锁，只是告诉调用过wait方法的线程可以去参与获得锁的竞争了，但不是马上得到锁，因为锁还在别人手里，别人还没释放。如果notify方法后面的代码还有很多，需要这些代码执行完后才会释放锁 
【总结】notify是告诉wait()的线程什么时候可以去继续去申请锁了 

#### java内存
大多数 JVM 将内存区域划分为 Method Area（Non-Heap）（方法区） ,Heap（堆） , Program Counter Register（程序计数器） ,   VM Stack（虚拟机栈，也有翻译成JAVA 方法栈的）,Native Method Stack  （ 本地方法栈 ），其中Method Area 和  Heap 是线程共享的  ，VM Stack，Native Method Stack  和Program Counter Register  是非线程共享的。为什么分为 线程共享和非线程共享的呢?请继续往下看。

首先我们熟悉一下一个一般性的 Java 程序的工作过程。一个 Java 源程序文件，会被编译为字节码文件（以 class 为扩展名），每个java程序都需要运行在自己的JVM上，然后告知 JVM 程序的运行入口，再被 JVM 通过字节码解释器加载运行。那么程序开始运行后，都是如何涉及到各内存区域的呢？

概括地说来，JVM初始运行的时候都会分配好 Method Area（方法区） 和Heap（堆） ，而JVM 每遇到一个线程，就为其分配一个 Program Counter Register（程序计数器） ,   VM Stack（虚拟机栈）和Native Method Stack  （本地方法栈）， 当线程终止时，三者（虚拟机栈，本地方法栈和程序计数器）所占用的内存空间也会被释放掉。这也是为什么我把内存区域分为线程共享和非线程共享的原因，非线程共享的那三个区域的生命周期与所属线程相同，而线程共享的区域与JAVA程序运行的生命周期相同，所以这也是系统垃圾回收的场所只发生在线程共享的区域（实际上对大部分虚拟机来说知发生在Heap上）的原因。
**虚拟机参数**
-Xmx10240m：代表最大堆
 -Xms10240m：代表最小堆
 -Xmn5120m：代表新生代
 -XXSurvivorRatio=3：代表Eden:Survivor = 3    根据Generation-Collection算法(目前大部分JVM采用的算法)，一般根据对象的生存周期将堆内存分为若干不同的区域，一般情况将新生代分为Eden ，两块Survivor；    计算Survivor大小， Eden:Survivor = 3，总大小为5120,3x+x+x=5120  x=1024
新生代大部分要回收，采用Copying算法，快！
老年代 大部分不需要回收，采用Mark-Compact算法
#### jdbc 加载驱动方法
加载驱动方法
1.Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
2. DriverManager.registerDriver(new com.mysql.jdbc.Driver());
3.System.setProperty("jdbc.drivers", "com.mysql.jdbc.Driver");