![](http://img.blog.csdn.net/20150908154704495)

JVM内存空间主要包含了：java堆、方法区、java虚拟机栈、本地方法栈、程序计数器等

**java堆**：共享区域，所有的对象都是在堆上创建的。

**方法区：**共享区域，也叫永久代，主要存放要加载的类信息（修饰符，类名）、静态变量、final定义的常量、方法信息和类中的field。调用类的  getName/isInterface方法这些数据都是来自于方法区

**运行时常量池**：是方法区的一部分，用于存放编译时产生的符号引用、直接引用、字面常量。同时除了编译期的常量外，还有运行期的一些常量，比如String.intern\(\)方法，String类就维护了一个常量池

虚拟机栈：

参考文献：

[http://blog.csdn.net/zhangerqing/article/details/8214365](http://blog.csdn.net/zhangerqing/article/details/8214365)

[http://blog.csdn.net/suifeng3051/article/details/48292193](http://blog.csdn.net/suifeng3051/article/details/48292193)

