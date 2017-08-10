![](http://hi.csdn.net/attachment/201009/25/0_1285381395C6iW.gif)

JVM内存空间主要包含了：java堆、方法区、java虚拟机栈、本地方法栈、程序计数器等

**java堆**：所有的对象都是在堆上创建的。

**方法区：**也叫永久代，主要存放要加载的类信息（修饰符，类名）、静态变量、final定义的常量、方法信息和类中的field。调用类的getName/isInterface方法这些数据都是来自于方法区



参考文献：

[http://blog.csdn.net/zhangerqing/article/details/8214365](http://blog.csdn.net/zhangerqing/article/details/8214365)

[http://blog.csdn.net/suifeng3051/article/details/48292193](http://blog.csdn.net/suifeng3051/article/details/48292193)

