---
title: HDFS关键设计和优势
tags: Big Data
---

# NameNode数据放置
## 数据块信息维护
* 目录树保存每个文件的块id
* 每个数据块所在的节点信息
* 根据`DataNode汇 报的信息`动态维护位置信息
* 不会持久化数据块位置信息

## 数据放置策略
* 新数据存放到哪些节点
* 数据均衡需要怎样合理搬迁数据
* 3个副本怎样合理放置

# DataNode数据放置
## 数据块的硬盘存放
* 文件在NameNode已分隔成block
* DataNode以block为单位对数据进行存取
## 启动扫盘
* DataNode需要知道本机存放了哪些数据块
* 启动时把本机硬盘上的数据块列表加载到内存中

# HDFS写异常处理
- 租约：Client要修改一个文件时，需要通过NameNode上锁，这个锁就是租约
- 情景：文件写了一半，client自己挂掉了。
* 副本不一致
* Lease无法释放

1. Lease Recovery : 写时client挂
2. Pipeline Recovery : 写时DataNode挂


