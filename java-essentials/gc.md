## 学习资料
* [3篇博文](https://www.cubrid.org/blog/how-to-monitor-java-garbage-collection/)
* [G1官方教程](https://www.oracle.com/technetwork/tutorials/tutorials-1876574.html)
* [CMS官方文档](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/cms.html)
* [jstat官方文档](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/jstat.html)

# 垃圾回收的原理
### 垃圾回收器的假设
垃圾回收器的基本假设：
* 大部分对象很快就会没用
* 有用的对象会有用很久

所以GC设计成新生代(Young Generation)和老生代(Old Generation)。新建对象在新生代区域，过了一段时间还有用（被引用）的对象进入老生代区域。
新生代又细分为伊甸(Eden)，2个存活区（Survior).另外还有一个永生代，存放class。

### 完整的垃圾回收过程：
1. new的时候，进入新生代的伊甸区
2. 发生一次小GC(minor GC), 有用的对象从eden进入一个survivor区。
3. 重复2，直到survivor满了。然后移到另一个survivor区。
4. 重读3，N次以后，认为对象老了。有用的对象进入老生代。
5. 如果老生代满了，发生Full GC.


### 老生代垃圾回收器类型
* Serial GC  适合单核CPU，桌面应用
* Parallel GC
* Parallel Old GC
* Concurrent Mask&Sweep GC(CMS)  适合多核服务器
* Garbage First(G1) GC

命令行参数指定GC算法: -XX:+UseConcMarkSweepGC

### 老生代算法
标记-清除-紧凑（mark-sweep-compact）
1. 标记有用的对象
2. 删除没用的对象
3. 把有用的对象排队放在一起，剩下完整的空闲区域。

## 问答
### CMS哪些情况下promotion failed导致Full GC,描述下原因
答：minor GC过程中，Survivor的剩余空间不足以容纳Eden和当前Survivor区域的存活对象，然后移至老生代时发现老生代也不足。发生Full GC.

### 描述一下CMS的过程
答：
* 停止所有应用线程
* 初始标记所有根引用的对象，恢复所有应用线程
* 并发标记，从上一步标记的对象开始，标记处所有的对象。同时，跟踪所有并发标记期间引用变化的对象。
* 停止所有应用线程。重新对并发标记期间有引用变化的对象重新标记。恢复所有应用线程。
* 并发清除废弃的对象。
* 并发调整堆的可用空间大小。

# 垃圾回收的监控
GC是JVM内部行为，通过工具可以观察。

## 常用工具: 
jstat。

jstat –gc <vmid> 1000 //每1000毫秒刷新一次，打印GC数据。GC数据主要是每个区域（新，老，Eden,Survivor),永生代等区域的容量和使用量。以及minorGC， fullGC的计数和耗时。

## 缺点
jstat输出的GC耗时是平均值。不是每次GC的具体值。想要看更详细的，每一次GC的数据，需要使用-verbosegc开关

## 图形化监控
visualGC可以直观的观察每个区域的变化。远程使用需要jstatd配合。


# 垃圾回收性能调优
