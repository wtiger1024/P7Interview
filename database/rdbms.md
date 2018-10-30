# 关系数据库


## 事务
事务的4大特性： ACID， automicity,Consistency,Isolation,Durability.
* 原子是指操作的原子性，
* 一致是指事物内一组操作的原子性，
* 隔离指事物之间互不干扰，
* 持久指数据写入磁盘。

### 事务的隔离级别
* Read Uncommitted 读未提交（事务A可以读取事务2未提交的数据）
* Repeatable Read 可重复读（MySQL默认），事务A读取过的数据，即使事务B修改了，对事务A也没有影响，可重复读取。（MySQL采用快照的概念实现此隔离级别）。注意：可重复度仅仅是涉及读，并没有涉及删和改。
* Read Committed 读已提交（Oracle默认）事务A可以读取事务B已经提交的数据（也即不可重复读）
* Serialize 序列化，事务之间没有并发。

### Mysql为了实现可重复读，采用了MVCC(multi-version concurrent control)机制
每一行记录都有隐藏的2个字段（创建版本号，删除版本号）。这里的版本号指事务的编号（递增）。更新记录时，原记录标记为删除，删除版本号为当前版本号。然后新增记录（创建版本号为当前版本号）。任何一个事务，只查询小于等于当前事务版本号的记录，这样就实现了“快照”的功能。只有SELETCT才会读取快照数据，INSERT,UPDATE,DELETE不会使用快照数据。

[幻读](https://dev.mysql.com/doc/refman/8.0/en/innodb-next-key-locking.html）
幻读是在Repeatable Read的级别下，事务一更新了其它事务增删的记录。可重复读是读取了快照，所以能保证可重复读，但是其他写操作并不是修改快照。所以，更新的时候，依然会改到别的事务提交的数据。为了解决幻读，引入间隙锁。
    
## 锁
### 间隙锁(Gap lock)
innodb为了避免幻读，引入了[间隙锁](https://dev.mysql.com/doc/refman/5.7/en/innodb-locking.html#innodb-gap-locks). mysql的行级锁其实是基于索引记录实现的，间隙锁也是一样。

## 索引

## SQL优化

## 分库分表

## 连接池
