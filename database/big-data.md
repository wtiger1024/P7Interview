# 大数据
只需要了解大数据的基本概念

关键概念：
* hadoop 基于谷歌论文mapreduce的一个开源实现，一个分布式计算框架。
* map-reduce map就是把原始数据转换成图(Map)，比如把单词作为Key，次数作为Value。reduce就是缩减，比如把相同键值的value相加。
* hdfs 基于谷歌论文的开源实现。把大量计算机管理起来，做成分布式文件系统。允许部分节点损坏，由于数据有冗余，所以不会导致数据丢失。
* spark
* flink
* storm
