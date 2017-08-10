

JVM内存空间主要包含了：java堆、方法区、java虚拟机栈、本地方法栈、程序计数器等

* **java堆**：共享区域，所有的对象都是在堆上创建的。

* **方法区：**共享区域，也叫永久代，主要存放要加载的类信息（修饰符，类名）、静态变量、final定义的常量、方法信息和类中的field。调用类的getName/isInterface方法这些数据都是来自于方法区。

* **运行时常量池**：是方法区的一部分，用于存放编译时产生的符号引用、直接引用、字面常量。同时除了编译期的常量外，还有运行期的一些常量，比如String.intern\(\)方法，String类就维护了一个常量池

* **虚拟机栈：**线程私有，每个线程都对应一个虚拟机栈，占用操作系统内存，用于存放局部变量表、操作数栈、方法出口、动态链接、方法调用的中间结果及返回值。当一个方法调用时栈帧入栈，调用结束方法出栈。

  ```
   局部变量表是方法相关的局部变量，包括基本数据类型、对象引用、返回地址等。long和double类型会占用两个slot，其他都是 

   一个slot，局部变量表是在编译期就产生了，在方法生命周期内都不会改变。
  ```

* **本地方法栈：**用于执行本地native方法， 与虚拟机栈机制一样，只是虚拟栈是用于执行java方法，有些虚拟机如sun默认的Hotspot虚拟将虚拟栈和本地方法栈放在一起使用

* **程序计数器：**线程私有，当前线程的行号指示器，用于指示当前线程执行到第几行字节码。一个线程就一个程序计数器。字节码解析器通过修改计数器来获取下一条指令。如果执行的是java方法，则程序计数器记录的是当前执行的虚拟机字节码指令地址，如果执行的是本地方法（C语言），则记录的是undenfied。该区域不会内存溢出。

### Java7中的改变：

方法区中的符号引用放到了Native heap，字面常量\(String.intern\)和静态变量放到了java heap中

Native heap，就是C\_Heap，对于32位的机器C-Heap的容量=4G-Java Heap-PermGen，对于64位的JVM，C-Heap的容量=物理服务器的总RAM+虚拟内存-Java Heap-PermGen。Linux机器虚拟内存即为交换空间swap space

### Java8中的改变：

元空间：在java8中取消了方法区，用于存放类的元数据信息，

参考文献：

[http://blog.csdn.net/zhangerqing/article/details/8214365](http://blog.csdn.net/zhangerqing/article/details/8214365)

[http://blog.csdn.net/suifeng3051/article/details/48292193](http://blog.csdn.net/suifeng3051/article/details/48292193)

