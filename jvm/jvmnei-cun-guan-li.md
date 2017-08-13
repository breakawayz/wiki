![](/assets/jvm_mem.png)

![](http://s10.sinaimg.cn/bmiddle/65ca444fnbcf86b581fa9&690)

JVM内存空间主要包含了：java堆、方法区、java虚拟机栈、本地方法栈、程序计数器等

* **java堆**：共享区域，所有的对象都是在堆上创建的。`java7之后还包含静态变量和字符常量String.intern`

* **方法区：**共享区域，也叫永久代，主要存放要加载的类信息（修饰符，类名）、静态变量、final定义的常量\(每个final变量都在常量池有一份拷贝\)、方法信息和类中的field。调用类的getName/isInterface方法这些数据都是来自于方法区。`java8之后该区域移除，新增元空间存放类元数据`

* **运行时常量池**：方法区一部分，用于存放编译时产生的符号引用、直接引用、字面常量。同时除了编译期的常量外，还有运行期的一些常量，比如String.intern\(\)方法，String类就维护了一个常量池

* **虚拟机栈：**线程私有，每个线程都对应一个虚拟机栈，占用操作系统内存。每个方法执行都产生一个栈帧，栈帧用于存放局部变量表、操作数栈[^1]、方法出口、动态链接、方法调用的中间结果及返回值。当一个方法调用时栈帧入栈，调用结束方法出栈。

  * 局部变量表：是方法相关的局部变量，包括基本数据类型、对象引用、返回地址等。long和double类型会占用两个slot，其他都是 一个slot，局部变量表是在编译期就产生了，在方法生命周期内都不会改变。

  * 操作数栈：存放数据方式与局部变量表一样，只不过访问方式是通过压栈和出栈来操作，而不是局部变量表索引方式。java虚拟机将操作数栈作为工作区，大部分指令都是从这里弹出数据、执行运算、然后将结果压回栈中。也可以从其他地方比如常量池或者字节码流中跟随在操作码-指令字节后面的字节。虚拟机指令执行过程中会将局部变量表中索引的数据压入操作数栈中执行运算

  * 动态链接：用于将用字符串描述的符号引用转换实际内存地址的直接引用

  * 方法出口\(return address\)：无论是字节码指令返回还是异常返回，在方法返回之前都需要回到返回调用的位置，这样程序才能正常执行下去。方法返回时栈帧可能要保存一些信息，用于帮助它恢复上层执行的状态。方法退出时相当于把当前栈帧出栈，可能包含如下操作：恢复上层局部变量表和操作数栈，将当前返回结果压入操作数栈，调用pc计数器指向方法调用后面的一条指令。方法正常返回时调用pc计数器的值可以作为返回地址，因此栈帧可能保存的是这个值，异常退出时，返回地址有异常处理器指定，栈帧可能不会保存该值

* **本地方法栈：**用于执行本地native方法， 与虚拟机栈机制一样，只是虚拟栈是用于执行java方法，有些虚拟机如sun默认的Hotspot虚拟将虚拟栈和本地方法栈放在一起使用

* **程序计数器：**线程私有，当前线程的行号指示器，用于指示当前线程执行到第几行字节码。一个线程就一个程序计数器。字节码解析器通过修改计数器来获取下一条指令。如果执行的是java方法，则程序计数器记录的是当前执行的虚拟机字节码指令地址，如果执行的是本地方法（C语言），则记录的是undenfied。该区域不会内存溢出。

#### Native heap：

直接内存，非java运行是的一部分。nio类可以通过本地native函数库直接分配堆外内存，然后通过在堆内的DirectByteBuffer对象作为这块内存的应用操作这块内存。

### Java7中的改变：

方法区中的符号引用放到了Native heap，字面常量\(String.intern\)和静态变量放到了java heap中

Native heap，就是C\_Heap，对于32位的机器C-Heap的容量=4G-Java Heap-PermGen，对于64位的JVM，C-Heap的容量=物理服务器的总RAM+虚拟内存-Java Heap-PermGen。Linux机器虚拟内存即为交换空间swap space。

### Java8中的改变：

元空间：在java8中取消了方法区，用于存放类的元数据信息。在元空间中，类的元数据生命周期与类加载器的生命周期是一致的。当类加载不在存活时，垃圾收集器会扫描该元数据是否存在引用。

#### 元空间和方法区的区别：

1. 元空间的大小用户不用指定，默认无限制，受限于本地native memory大小。而方法区的大小用户必须指定，默认64M很容易溢出
2. 类和方法的大小很难估算，因此很难确定方法区的大小，太小容易溢出，太大容易导致老年代溢出（堆空间有限，此消彼长）
3. hotspot中的垃圾收集器有专门的代码负责方法区的管理。而元空间与java heap是在相同的地址空间中，元空间和java heap可以无缝管理，简化了FullGC的过程。将来也可以并行管理元数据
4. 类大部分都是static，很少被收集或者卸载，因此方法区的垃圾收集不仅增加了复杂度而且效率低

参考文献：

1、[http://denverj.iteye.com/blog/1218359](http://denverj.iteye.com/blog/1218359)

[http://blog.csdn.net/zhangerqing/article/details/8214365](http://blog.csdn.net/zhangerqing/article/details/8214365)  Java之美\[从菜鸟到高手演变\]之JVM内存管理及垃圾回收

[http://blog.csdn.net/suifeng3051/article/details/48292193](http://blog.csdn.net/suifeng3051/article/details/48292193)  JVM内存管理及GC机制

[http://blog.csdn.net/zhushuai1221/article/details/52122880](http://blog.csdn.net/zhushuai1221/article/details/52122880)

[http://www.cnblogs.com/niejunlei/p/5987611.html](http://www.cnblogs.com/niejunlei/p/5987611.html)**   **Java虚拟机栈

[https://my.oschina.net/u/1156843/blog/203442](https://my.oschina.net/u/1156843/blog/203442) 深入理解java虚拟机-运行时栈帧结构

[https://blog.smoker.cc/java/learn-jvm-1.html](https://blog.smoker.cc/java/learn-jvm-1.html) 《深入理解Java虚拟机》学习笔记-1

