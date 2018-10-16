## 学习资料
* [3篇博文](https://www.cubrid.org/blog/how-to-monitor-java-garbage-collection/)
* [G1官方教程](https://www.oracle.com/technetwork/tutorials/tutorials-1876574.html)

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
问：CMS哪些情况下promotion failed导致Full GC,描述下原因

答：minor GC过程中，Survivor的剩余空间不足以容纳Eden和当前Survivor区域的存活对象，然后移至老生代时发现老生代也不足。发生Full GC.

# 垃圾回收的监控


# 垃圾回收性能调优
