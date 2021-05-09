---
title: 【大数据基础】Hadoop入门--HDFS
type: categories
copyright: fals
date: 2019-11-27 14:16:49
tags: Hadoop
categories: 大数据技术
title_en: Hadoop_02
---


> 本系列为《Hadoop大数据处理基础与实践》的读书笔记。

# 一，Hadoop 概述

## 1，`Hadoop` 来源和动机

`Hadoop` 用 `Java` 语言开发，是对 `Google` 的 `MapReduce`、`GFS`(`Google File SZstem`)和 `Bigtable` 等核心技术的开源实现，由 `Apache` 软件基会支持，是以 `Hadoop` 分布式文件系统 (`Hadoop Distributed File System,HDFS`)和 `MapReduce`(`Google MapReduce`)为核心,以及 一些支持 `Hadoop` 的其他子项目的通用工具组成的分布式计算系统，主要用于海量数据(大于 1`TB`)高效的存储、管理和分析。

![Hadoop Logo](/images/hadoop_20191112144932.png)

`Hadoop` 最起源于 `Nutch`。`Nutch` 是基于 `Java` 实现的开源搜索引擎，2002年由 `Doug Cutting` 领衔的 `Yahoo` 团队开发。2003年，`Google` 在 `SOSP`(操作系统原理会议)上发表了有关 `GFS`(`Google File SZstem`，`Google` 文件系统)分布式存储系统的论文；2004 年，`Google` 在 `OSDI`（操作系统设计与实现会议）上发表了有关 `MapReduce` 分布式处理技术的论文。

![Hadoop发展历程](/images/hadoop_20191112145704.png)

![Hadoop发展时序图](/images/hadoop_20191112145746.png)

`Hadoop` 是基于以下思想设计的：

> （1）可以通过普通机器组成的服务器群来分发以及处理数据，这些服务器群总计可达数千个节点，使高性能服务成本极度低（`Economical`）。
>
> （2）极度减小服务器节点ܾ失效导致的问题，不会因某个服务器节点ܾ失效导致工作不能正常进行，能实现该方式的原因是 `Hadoop` 能自动地维护数据的多份复制，并且在任务失败后能自动地重新部署计算任务，实现了工作可靠性（`Reliable`）和弹性扩容能力（`Scalable`）。
>
> （3）能高效率（`Efficient`）地存储和处理千兆字节（`PB`）的数据，通过分发数据，`Hadoop` 可以在数据所在的节点上并行地处理它们，这使得处理非常的快速。
>
> （4）文件不会被频繁写入和修改；机柜内的数据传输速度大于机柜间的数据传输速度； 海量数据的情况下移动计算比移动数据更高效（`Moving Computation is Cheaper than Moving Data`）。

## 2，`Hadoop` 体系架构

`Hadoop` 实现了对大数据进行分布式并行处理的系统框架，是一种数据并行方法。由实现数据分析的 `MapReduce` 计算框架和实现数据存储的分布式文件系统 `HDFS` 有机结合组成，它自动把应用程序分成许多小的工作单元，并把这些单元放到集群中的相应节点上执行，而分布式文件系统 `HDFS` 负责各个节点上的数据的存储，实现高吞吐率的数据读写。`Hadoop` 的基础架构如图所示。

![Hadoop基础架构](/images/hadoop_20191118224333.png)

`MapReduce` 的主要吸引力在于：它支持使用廉价的计算机集群对规模达到 `PB` 级的数据集进行分布式并行计算，是一种编程模型。它由 `Map` 函数和 `Reduce` 函数构成，分别完成任务的分解与结果的汇总。`MapReduce` 的用途是进行批量处理，而不是进行实时查查询，特别不适用于交互式应用。

`HDFS` 中的数据具有“**一次写，多次读**”的特征，即保证一个文件在一时刻只能被一个调用者执行写操作，但可以被多个调用者执行读操作。目前，`Hadoop` 已经发展成为包含很多项目的集合，形成了一个以 `Hadoop` 为中心的生态系（`Hadoop Ecosystem`），如图所示。

![Hadoop生态系统圈](/images/hadoop_20191112151532.png)

> - `ETL Tools` 是构建数据仓库的重要环节，由一系列数据仓库采集工具构成。
>- `BI Reporting`（`Business Intelligence Reporting`，商业智能报表）能提供综合报告、数据分析和数据集成等功能。
> - `RDBMS` 是关系型数据库管理系统。`RDBMS` 中的数据存储在被称为表（`table`）的数据库中。表是相关的记录的集合，它由列和行组成，是一种二维关系表。
>- `Pig` 是数据处理脚本，提供相应的数据流（`Data Flow`）语言和运行环境，实现数据转换（使用管道）和实验性研究（如快速原型），适用于数据准备阶段。`Pig` 运行在由 `Hadoop` 基本架构构建的集群上。
> - `Hive` 是基于平面文件而构建的分布式数据仓库，擅长于数据展示，由 `Facebook` 贡献。`Hive` 管理存储在 `HDFS` 中的数据，提供了基于 `SQL` 的查询语言（由运行时的引擎翻译成 `MapReduce` 作业）查询数据。`Hive` 和 `Pig` 都是建立在 `Hadoop` 基本架构之上的，可以用来从数据库中提取信息，交给 `Hadoop` 处理。
> - `Sqoop` 是数据接口，完成 `HDFS` 和关系型数据库中的数据相̈转移的工具。
> - `HBase` 是类ͪ于 `Google BigTable` 的分布式列数据库。`HBase` 和 `Avro` 于 2010 年 5 月成为顶级 `Apache` 项目。`HBase` 支持 `MapReduce` 的并行计算和点查询（随机读取）。
> - `Avro` 是一种新的数据序列化（`serialization`）格式和传输工具，主要用来取代 `Hadoop` 基本架构中原有的 `IPC` 机制。
> - `Zookeeper` 用于构建分布式应用，是一种分布式锁设施，提供类ͪ 似 `Google Chubby`（主要用于解决分布式一致性问题）的功能，它是基于 `HBase` 和 `HDFS` 的，由 `Facebook` 贡献。
> - `Ambari` 是最新加入 `Hadoop` 的项目， `Ambari` 项目旨在将监控和管理等核心功能加入 `Hadoop` 项目。`Ambari` 可帮助系统管理员部署和配置 `Hadoop`、升级集群以及监控服务。
> - `Flume` 是 `Cloudera` 提供的一个高可用的、高可靠的、分布式的海量日志采集、聚合和传输的系统，`Flume` 支持在日志系统中定制各类数据发送方，用于收集数据；同时，`Flume` 提供对数据进行简单处理，并写到各种数据接受方（可定制）的能力。
> - `Mahout` 是机器学习和数据挖掘的一个分布式框架，区别于其他的开源数据挖掘软件，它是基于 `Hadoop` 之上的；`Mahout` 用 `MapReduce` 实现了部分数据挖掘算法，解决了并行挖掘的问题，所以 `Hadoop` 的优势就是 `Mahout` 的优势。

关系型数据库与`MapReduce`的比较如下：

|          | 关系型数据库   | `MapRedude`        |
| :------- | -------------- | ------------------ |
| 数据大小 | GB             | PB                 |
| 数据存取 | 交互式和批处理 | 批处理             |
| 更新     | 多次读/写      | 一次写入，多次读取 |
| 事务     | ACID           | 无                 |
| 结构     | 写时模式       | 读时模式           |
| 完整性   | 高             | 低                 |
| 横向扩展 | 非线性         | 线性               |

# 二，HDFS技术

## 1，`HDFS` 的特点

1）简单一致性：对 `HDFS` 的大部分应用都是一次写入多次读（只能有一个 `writer`，但可以有多个 `reader`）， 如搜索引擎程序，一个文件写入后就不能修改了。因此写入 `HDFS` 的文件不能修改或编辑， 如果一定要进行这样的操作，只能在 `HDFS` 外修改好了再上传；

2）故障检测和自动恢复：`HDFS` 具有容错性（`fault-tolerant`），能够自动检测故障并迅速恢复，因此用户察觉不到明显的中断；

3）流式数据访问：`Hadoop` 的访问模式是一次写多次读，而读可以在不同的节点的冗余副本读，所以读数据的时间相应可以非常短，非常适合大数据读取。运行在 `HDFS` 上的程序必须是流式访问数据集，接着长时间在大数据集上进行各类分析，所以 `HDFS` 的设计旨在提高数据吞吐量，而不是用户交互型的小数据；

4）支持超大文件：由于更高的访问吞吐量，`HDFS` 支持 `GB` 级甚至 `TB` 级的文件存储，但如果存储大量小文件的话对主节点的内存影响会很大；

5）优化的读取：由于 `HDFS` 集群往往是建立在跨多个机架（`RACK`）的集群机器上的，而同一个机架节点间的网络带宽要优于不同机架数据块进行复制。

## 2，`HDFS` 架构

`HDFS` 是一个典型的主从架构，一个主节点或者说是元数据节点（`MetadataNode`）负责系统命名空间（`NameSpace`）的管理、客户端文件操作的控制和存储任务的管理分配，多个从节点或者说是数据节点（`DataNode`）提供真实文件数据的物理支持，系统架构如图所示。

![HDFS架构图](/images/hadoop_20191112164611.png)

在 `HDFS` 上，块默认为64 `MB`。在 `HDFS` 上的文件被划分成多个64 `MB` 的大块（`Chunk`）作为独立储存单元。与单机分布式文件系统不同的是，不满一个块大小的数据不会占据整个块空间，也就是这个块空间还可以给其他数据共享。设置块大小目的是把寻址时间占所有传输数据所用的时间最小化，即增大实际传输数据的时间。

- 检查 `HDFS` 系统上 `input` 目录下的数据块的健康状况

```bash
hdfs fsck /input -blocks -files
```

`HDFS` 集群有两种按照主（`master`）从（`slave`）模式划分的节点：元数据节点（`MetadataNode`） 和数据节点（`DataNode`）。

元数据节点负责管理整个集群的命名空间，并且为所有文件和目录维护了一个树状结构的元数据信息，而元数据信息被持久化到本地磁盘上分别对应了两种文件：文件系统镜像文件（`FsImage`）和编辑日志文件（`EditsLog`）。文件系统镜像文件存储所有关于命名空间的信息，编辑日志文件存储所有事务的记录。一般会配置两个目录来存储这两个文件，分别是本地磁盘和网络文件系统（`NFS`），防止元数据节点所在节点磁盘损坏后数据丢失。元数据节点在磁盘上的存储结构如下所示。

![元数据节点在磁盘上的存储结构](/images/hadoop_20191112172402.png)

编辑日志文件会随着事务操作的增加而增大，所以需要把编辑日志文件合并到文件系统镜像文件当中去，这个操作就由**辅助元数据节点**（`Secondary MetadataNode`）完成。

**辅助元数据节点**主要工作是周期性地把文件系统镜像文件与编辑日志文件合并，然后清空旧的编辑日志文件。

![辅助元数据节点工作原理](/images/hadoop_20191118235328.png)

辅助元数据节点加载磁盘上的文件系统镜像文件和编辑日志文件，在内存中合并后成为新的文件系统镜像文件，然后写到磁盘上，这个过程称作**保存点**（`CheckPoint`），合并生成的文件为 `fsimage.ckpt`。

当元数据节点启动时，会将文件系统镜像载入内存，并执行编辑日志文件中的各项操作，然后开始监控 `RPC` 和 `HTTP` 请求，此时会进入到一种特殊状态，即**安全模式状态**（`Safe Mode`）。

- 查看系统是否处于安全模式：

```bash
hdfs dfsadmin -safemode get
```

- 进入安全模式：

```bash
dfs dfsadmin -safemode enter
```

- 离开安全模式：

```bash
dfs dfsadmin -safemode level
```

- 在执行某个脚本之前先退出安全模式：

```bash
dfs dfsadmin -safemode wait
```

在 `HDFS` 中，`ReplicationTargetChooser` 类是负责实现为新分配的数据块ࠬ寻找最优存储位置的。总体说，数据块的分配工作和备份的数量、申请的客户端地址，已注册的数据服务器位置密切相关。其算法基本思路是只考量静态位置信息，优先照顾写入者的速度，让多份备份分配到不同的机架去。此外，`HDFS` 中的 `Balancer` 类是为了实现动态的负载调整而存在的。 `Balancer` 类派生于 `Tool` 类，这说明它是以一个独立的进程存在的，可以独立的运行和配置。它运行有 `NamenodeProtocol` 和 `ClientProtocol` 两个协议，与主节点进行通信，获取各个数据服务器的负载状况，从而进行调整。主要的调整其实就是一个操作，将一个数据块从一个服务器搬迁到另一个服务器上。

使用负载均衡的命令：

```bash
hadoop balance [-threshold<threshold>]  # []为可选参数，默认阈值为10%，代表磁盘容量的百分比。
```

对分布式文件系统而言，没有利用͈价值的数据块备份，就是**垃圾**。基本上， 所有的垃圾都可以视为两类：

> 一类是由系统正常逻辑产生的，如一个文件被删除了，所有相关度数据块都沦为了垃圾，或一个数据块被负载均衡器移动了，原始数据块也不幸成了垃圾。
>
> 另一类垃圾是由于系统的一些异常症状产生的，如一个数据服务器停机了一段，重启之后发现其上的一个数据块已经在其他服务器上重新增加了此数据块的备份，它上面的备份因过期而ܾ失去了价值，就需要当作垃圾来处理。

## 3，`HDFSShell`命令

 `hdfs URI` 格式：`scheme://authority:path`。

其中，`scheme` 表示协议名，可以是 `file` 或 `HDFS`，前者是本地文件，后者是分布式文件；`authority` 表示集群所在的命名空间；`path` 表示文件或者目录的路径。

- `hdfs dfs`命令大全

![HDFS文件系统命令](/images/hadoop_20191119100234.png)

- `hdfs dfsadmin` 命令大全

![dfsadmin命令大全](/images/hadoop_20191119100802.png)

- `namenode`命令大全

![namenode命令](/images/hadoop_20191119101049.png)

- `fsck`命令大全

![fsck命令](/images/hadoop_20191119102026.png)

- `pipes`命令大全

![pipes命令](/images/hadoop_20191119101656.png)

- `job`命令大全

![job命令](/images/hadoop_20191119101826.png)

## 4，`HDFS`中的`Java API`的使用

`Hadoop`的文件系统如下：

| 文件系统   | URI方案 | Java实现                       | 定义                                                         |
| ---------- | ------- | ------------------------------ | ------------------------------------------------------------ |
| Local      | file    | fs.LocalFileSystem             | 支持有客户端校验和本地文件系统。带有校验和的本地系统文件在`fs.LocalFileSystem`中实现 |
| HDFS       | hdfs    | hdfs.DistributionFileSystem    | `Hadoop`的分布式文件系统                                     |
| HFTP       | hftp    | hdfs.HftpFileSystem            | 支持通过`HTTP`方式以只读方式访问`HDFS`，`distcp`经常用在不同的`HDFS`集群间复制数据 |
| HSFTP      | hsftp   | hdfs.HsftpFileSystem           | 支持通过`HTTPS`方式以只读方式访问`HDFS`                      |
| HAR        | har     | fs.HarFileSystem               | 构建在`Hadoop`文件系统之上，对文件进行归档，以减少`NameNode`的内存使用 |
| KFS        | kfs     | fs.kfs.KosmosFileSystem        | `Cloundstore`（前身是`Kosmos`文件系统）文件系统是类似于`HDFS`和`Google`的`GFS`文件系统，使用`C++`编写 |
| FTP        | ftp     | fs.ftp.FtpFileSystem           | 由`FTP`服务器支持的文件系统                                  |
| S3(本地)   | s3n     | fs.s3native.NativeS3FileSystem | 基于`Amazon S3`的文件系统                                    |
| S3(基于块) | s3      | fs.s3.NativeS2FileSystem       | 基于`Amazon S3`的文件系统，以块格式存储解决了`S3`的`5GB`文件大小限制 |

## 5，`RPC`通信

`RPC`（`Remote Procedure Call Protocol`）即远程调用协议，是一台计算机通过跨越底层网络协议（`TCP`、`UDP` 等）调用另一台计算机的子程序或者服务所遵守的协议标准。其主要特点如下：

> 1）透明性：远程调用其他机器上的程序，对用户来说就像是调用本地方法一样；
>
> 2）高性能：`RPC server` 能够并发处理多个来自 `Client` 的请求；
>
> 3）可控性：`JDK` 中已经提供了一个 `RPC` 框架——`RMI`，但是该 `PRC` 框架过于重量级 并且可控之处比较少，所以 `Hadoop RPC` 实现了自定义的 `PRC` 框架。

