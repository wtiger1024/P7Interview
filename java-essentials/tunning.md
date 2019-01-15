## 学习资料
* [官方介绍文档](https://www.oracle.com/technetwork/java/javaseproducts/mission-control/java-mission-control-wp-2008279.pdf)

# JMC, JFR
### JFR Java Flight Recorder 飞行记录仪
垃圾回收器的基本假设：
* 飞行记录仪主要用于记录JVM内部的事件
* JFR可以记录到文件，然后由JMC读取，展示
* JMC是基于JMX的控制台，展示JFR记录的数据

# disruptor, a high performance queue
* [lmax disruptor doc](http://lmax-exchange.github.io/disruptor/files/Disruptor-1.0.pdf)
优化思路
*  重用对象，减少GC
* false-sharing. 缓存padding，避免变量共享cache line
* 避免使用lock。尽量使用自旋锁，yield, parkNano等OS、提供的调度机制。
