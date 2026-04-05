
![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/java/jvm.png)

Metaspace（元空间）是用于存放所有被加载类的元数据信息的内存区域

Heap（堆）是一块用于存储对象实例的内存区域。它是Java虚拟机中最大的一块内存区域，也是所有线程共享的内存区域。

堆在Java虚拟机中具有以下特点：

1. 存储对象实例：堆主要用于存储动态创建的对象实例。在Java程序运行过程中，通过new关键字等方式创建的对象都会存储在堆上。堆中的对象实例由垃圾回收器负责管理和回收。

2. 自动内存管理：堆是自动内存管理的一部分，Java虚拟机负责自动分配、释放堆内存空间以及对象的生命周期管理。通过垃圾回收算法，释放不再被引用的对象所占用的内存空间。

3. 分代结构：堆可以根据对象的存活时间划分为不同的代（Generation），通常将堆划分为新生代（Young Generation）和老年代（Old Generation）。新生代用于存储新创建的对象，而老年代则用于存储存活时间较长的对象。

4. 堆扩展与压缩：堆可以根据需要进行扩展，当堆中的空间不足时，Java虚拟机会通过堆扩展机制扩大堆内存空间。此外，还存在堆的压缩机制，用于减少堆内存空间的碎片，提高内存的利用率。

5. 线程共享：堆是所有线程共享的内存区域，不同线程可以同时访问和操作堆中的对象。

Stack（栈）是一块线程私有的内存区域，用于存储方法调用过程中的局部变量、操作数栈、返回值等信息。每个线程在执行方法时都会创建对应的栈帧（Stack Frame），用于保存当前方法的状态以及执行上下文信息。

栈在Java虚拟机中具有以下特点：

1. 存储方法调用信息：栈主要用于存储方法调用信息，包括方法的参数、局部变量、操作数栈、返回值等信息。每当一个方法被调用时，Java虚拟机就会为该方法创建一个新的栈帧，并将其压入栈顶。

2. 线程私有：每个线程都拥有自己独立的栈空间，不同线程之间的栈空间相互独立，互不干扰。

3. 后进先出：栈是一种后进先出的数据结构，即最后压入栈的元素会最先弹出。在Java中，栈的实现使用了链表的方式，每个栈帧都包含一个指向前一个栈帧的指针。

4. 自动内存管理：栈的生命周期与线程相同，当线程结束时，栈中的所有栈帧都会被自动销毁。因此，栈不需要进行垃圾回收，也没有内存泄漏的风险。

PC Registers 指的是程序计数器寄存器（Program Counter Registers）。每个线程都有自己的程序计数器寄存器，用于存储当前线程正在执行的字节码指令的地址或者索引。

Native Method Stack（本地方法栈）是用于执行本地方法（Native Method）的内存区域。

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/java/heap-structure.png)


Young Generation（年轻代）

Old Generation（年老代）

stop-the-world events（STW事件）：应用程序所有线程必须阻塞，直到垃圾回收操作完成之后才能继续执行。

> GC垃圾回收就会触发stop-the-world events

对象在Heap中的存放顺序一般是Eden区--> Survivor区 --> Old Generation

对象在Java堆中的存放顺序通常可以遵循以下的过程：

Eden区：当我们创建一个新的对象时，它首先会被分配到Eden区。Eden区是一个较小的内存空间，用于存放新创建的对象。

Survivor区（幸存者区）：如果对象在Eden区经历了一次垃圾回收后仍然存活，那么它就会被移动到Survivor区。Survivor区由两部分组成，通常称为S0（from）和S1（to）区域。在这两个区域中，一部分区域会被标记为可用空间，另一部分用于存放已经存活的对象。

幸存者区之间的复制：当进行垃圾回收时，存活的对象会从一个Survivor区（如S0）复制到另一个空闲的Survivor区（如S1），同时会对对象进行年龄标记。如果一个对象经历了多次垃圾回收后仍然存活，它将会被移动到年老代（Old Generation）。

Old Generation（年老代）：在对象经历多次垃圾回收后仍然存活时，它会被晋升到年老代。年老代是一个较大的内存空间，用于存放长时间存活的对象。

 

Minor GC、Major GC和Full GC

- Eden is getting full. So the higher the allocation rate, the more frequently Minor GC occurs.

- Major GC cleaning the Old Generation space.

- Full GC is cleaning the entire Heap – both Young and Old spaces.

> 较新的 JDK（如 JDK 11+）之后，随着 G1 成为默认收集器，通常只区分 Young GC 和 Mixed GC / Full GC。

在JVM优化中，降低垃圾回收（GC）的频率通常是一个重要的着手点，因为频繁的垃圾回收会导致程序的停顿时间增加，影响应用程序的性能和响应能力。结合Arthas工具，即可实时查看应用 load、内存、gc、线程的状态信息。

> https://arthas.aliyun.com/doc/