学习资料
* [3篇博文](https://www.cubrid.org/blog/how-to-monitor-java-garbage-collection/)
* [G1官方教程](https://www.oracle.com/technetwork/tutorials/tutorials-1876574.html)


==垃圾回收器的假设==
垃圾回收器的基本假设：
* 大部分对象很快就会没用
* 有用的对象会有用很久

所以GC设计成新生代(Young Generation)和老生代(Old Generation)。新建对象在新生代区域，过了一段时间还有用（被引用）的对象进入老生代区域。
新生代又细分为伊甸(Eden)，2个存活区（Survior).另外还有一个永生代，存放class。

整个对象的生命周期如下：
# new的时候，进入新生代的伊甸区
# 发生一次小GC(minor GC), 有用的对象从eden进入一个survivor区。
# 重复2，直到survivor满了。然后移到另一个survivor区。
# 重读3，N次以后，认为对象老了。有用的对象进入老生代。
# 如果老生代满了，发生Full GC.


