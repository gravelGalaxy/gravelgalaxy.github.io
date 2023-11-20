---
title: HDFS核心组件与作用
tags: Big Data
---

Client/SDK<---------->`NameNode`(Active Standby)
|                           |
|                           |
|                           |
|                           |
|                           |
<-------------DataNode-------->

# 写流程
1. Client 向NameNode请求写入数据块,问NameNode写到哪里
2. NameNode返回目标DataNode`列表`pipline
3. Client发送complete请求

# 读流程
1. Client向NameNode询问位置，NameNode记录了`块`在哪些DataNode节点


# 元数据节点NameNode
1. 维护目录树
目录树增删改查
2. 维护文件和数据快的关系
文件会被分为多个块,文件以数据块为单位进行多副本存放
3. 维护文件块存放节点的位置
通过接受DataNode的心跳汇报信息，维护集群节点的拓扑结构和每个文件块所有副本所在的DataNode列表。
4. 分配新文件存放节点

# 数据节点DataNode
1. 与Client：数据快存取
2. 与NameNode：心跳汇报
3. 副本复制

