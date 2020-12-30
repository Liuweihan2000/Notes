## Hadoop

### 解决问题：

- 海量数据的存储（HDFS）
- 海量数据的分析（MapReduce）
- 资源管理调度（YARN）

作者：Doug Cutting

受Google三篇论文的启发: GFS, MapReduce, BigTable

MapReduce没有运算逻辑，逻辑是我们自己写的，它只是来运行这些逻辑

YARN的功能：

把很多jar包分配到不同的机器上去

每个机器上分配CPU和内存资源，不管你的java程序里面具体是什么

一个虚拟的容器



Hadoop集群既可以用来做离线分析，也可以做storm的流式计算

Hadoop擅长离线日志分析

###  数据存在哪？

HDFS就是用来存储海量数据的，是一个分布式存储系统，把数据存在很多节点上



### 一个搜索引擎的例子

爬虫去爬页面，然后将页面存储起来

用Lucene将计算逻辑分配给分布式存储系统来建立索引

我们查询的时候查的就是这些索引



###  HDFS Architecture

![image-20201004163330496](C:\Users\liuwe\AppData\Roaming\Typora\typora-user-images\image-20201004163330496.png)

把文件切块，把每一块分配到不同的节点上

为了容错，每一块有多个副本在多台机器上

比如说1号块有三份副本，在三台机器上

这样做的好处是可以容错，也更好应对并发



这些操作对于客户端是透明的

客户端只需指定一个路径即可

这个路径和这个文件的块的存放位置之间应该有一些映射关系

这些映射关系由Namenode来管理

所以客户端访问的时候，首先访问Namenode



### 怎样解决海量数据的计算？

假设有一个文件特别大，1个T，把它分成很多个块

当我们把文件切块后，我们如何进行数据库的操作count*呢？

解决方案1：把每一块分别取出，放在内存或外存中不现实，因为把块取出的过程走的也是网络，需要占用带宽。

而且只有一台机器在工作，效率低下。

解决方案2：把处理的逻辑分发到数据所在的机器上面，每一个Datanode都可以统计自己机器上的count*

这样有1000个节点的话，1000个节点都在计算

这样每个Datanode都只知道自己节点统计出来了多少条数据，而不知道全局统计出来了多少数据



把程序分为两个阶段：Map + Reduce

Map程序对本地局部进行处理，每个节点都要执行

Reduce只在一台机器上面运行，需要通过网络所有节点生成的节点数目，虽然只有一个节点在运行，但是并不会影响并发度，也不会成为性能的瓶颈。这个节点只是在每个节点上取**一个**数而已，这个运算是几乎一瞬间的事情



我们可以设置多个Reduce，比如统计每个月的数据量，这样就设置12个Reduce



### HDFS

分布式文件系统那么多

为什么hadoop项目中还要开发一个hdfs文件系统？

区别在于：hdfs可以更好的支持分布式**计算**

学习hdfs的时候要本着一个思想：它怎么支持分布式计算？

### 存储模型

![image-20201004172558865](C:\Users\liuwe\AppData\Roaming\Typora\typora-user-images\image-20201004172558865.png)

只要是文件，它就是一个字节数组

线性 + 偏移量：能拼回来

id是用来映射的

按字节切一定会切坏，后期也一定会修复，一个中文字符占3个char，很有可能切坏

hdfs知道无论自己怎么切割块都不能满足所有的需求，所以需要使用者自己去设定切割的粒度

默认块大小：64MB --> 128MB --> 256MB

在以前IO比较慢，所以现在的默认块大小在越来越大

运维可以根据集群的性能手动去调整

location：块在哪个地址

块分着丢出去，还得知道它在哪

说某块有三个副本，那总共就只有三份文件，因为没有副本主从的概念

副本是用来满足可靠性和性能的关键

后三句话：

其实HDFS上传一份文件之后可以修改

客户端就想读取某个文件

当把某段读过来以后，想要插入某段文本

带来什么问题？

后续的块的偏移量都不对了

如果想修改，那将是一个泛洪的场景，所有的节点都将参与到消息的传递和修改中

所以如果想要HDFS支持修改，那必将影响其性能

所以HDFS不支持修改

追加数据就是增加新的块



回答之前的问题：设计的时候要很好的支持计算层



上传的时候会有校验，不会写错，每个文件在存的时候会有一个校验和



![image-20201004193619232](C:\Users\liuwe\AppData\Roaming\Typora\typora-user-images\image-20201004193619232.png)

主从 / 主备 是两个概念

由一个Namenode和一些Datanode组成

元数据：在Windows下就是文件的“属性”

NameNode有负载均衡的能力



Linux目录树对磁盘分区做了一个解耦，或者叫映射



![image-20201004195029327](C:\Users\liuwe\AppData\Roaming\Typora\typora-user-images\image-20201004195029327.png)

文件副本数可以不一样

DataNode一般不要超过5000台



![image-20201004195152501](C:\Users\liuwe\AppData\Roaming\Typora\typora-user-images\image-20201004195152501.png)

NameNode需要快速对外提供服务，所以存在内存里

内存存储是易失的，掉电就没

需要持久化方案，什么持久化方案？

每种使用内存的软件的方案都不一样，Redis



数据并不是HDFS存的，它只是管理映射，所以说HDFS不是一个数据库



### 数据持久化：

方案1

日志：记录实时发生的增删改的操作 mkdir /abc

可以用来恢复内存

优点：完整性比较好

缺点：加载恢复数据是时候特别慢、所占空间大



比如说，我NameNode所占内存是4G，已经成功运行了10年，日志文件有多大？

日志是追加的，是一个文本文件

花五年时间来恢复数据，内存会不会溢出？

不会，因为我们一直在创建/删除目录



方案2

镜像 / 快照 / dump / db

日志是实时发生的，而这些是间隔的

内存全量数据基于某一个时间点做的向磁盘的溢写



java序列化：把内存中的一个对象存入外存中的一个数组中



请问：999999999 占用多大的磁盘空间？

用什么文件编码？

txt byte 9个字节

int a = 999999999; int类型占用四个字节

byte类型是数据反序列化后能看懂，但int类型的就看不懂了

一般称后者为二进制文件



优点：恢复速度快过日志文件

缺点：因为是间隔的，容易丢失一部分数据



HDFS：两种技术同时使用，其中：

日志叫做Editslog

镜像叫做FsImage



Editslog：体积小，记录少的时候有优势

FsImage：如果能更快的更新时点（小时级、半小时级）

如何实现？ 

最近时点的FsImage + 增量的Editslog

现在10点：

FI：9点  + 9点~10点的增量的EL

1. 加载FI
2. 加载Editslog
3. 内存就得到了关机前的全量数据

![image-20201004201612369](C:\Users\liuwe\AppData\Roaming\Typora\typora-user-images\image-20201004201612369.png)



问题：那么FI时点是怎么滚动更新的？

要么由NN 8点溢写一次，9点溢写一次

要么NN：第一次开机的时候只写一次FI，到九点的时候，EL记录的是8~9点做过的事情

只需要将8~9点的日志的记录更新到8点的FI中，FI中的数据时点就变成了9点

需要另外一台机器来做

![image-20201004202032895](C:\Users\liuwe\AppData\Roaming\Typora\typora-user-images\image-20201004202032895.png)

![image-20201004202239419](C:\Users\liuwe\AppData\Roaming\Typora\typora-user-images\image-20201004202239419.png)

![image-20201004202012059](C:\Users\liuwe\AppData\Roaming\Typora\typora-user-images\image-20201004202012059.png)

Ha高可用

![image-20201004203423808](C:\Users\liuwe\AppData\Roaming\Typora\typora-user-images\image-20201004203423808.png)

NN存元数据：文件属性 / 每个块存在哪个DN上

在持久化的时候：文件属性会持久化，但是文件的每一块不会持久化

恢复的时候：NN会丢失块的位置信息

为什么要这么设计？

为了保持数据的一致性

重启时可能某个DN就挂掉了，这样客户端会访问错误的DN



### 副本放置策略

![image-20201004204202498](C:\Users\liuwe\AppData\Roaming\Typora\typora-user-images\image-20201004204202498.png)

早起版本第二个节点同机架，第三个节点才出机架

后来才更改过来

为什么？

因为副本的数量可能为2

![image-20201004204554124](C:\Users\liuwe\AppData\Roaming\Typora\typora-user-images\image-20201004204554124.png)

不是客户端和三个DN同时建立TCP连接

Pipeline流水线工作，流式传输数据

为什么要切割？学过计网的都知道

块仍然能分割成pkt

pkt分割成chunk  512字节

流水线也是一种变种的并行

好处：副本的数量对于客户端来说是透明的，客户端只用传一份就行了



虽然Client在存数据之前会告诉NN存储的方法，但NN只相信真实存货的DN向他汇报的数据

如果这个过程中的某个DN挂掉了，NN会只收到两个DN的信息，他就知道有一个DN挂掉了，所以它会命令DN再复制出一个块来

![image-20201004205545159](C:\Users\liuwe\AppData\Roaming\Typora\typora-user-images\image-20201004205545159.png)

![image-20201004210513824](C:\Users\liuwe\AppData\Roaming\Typora\typora-user-images\image-20201004210513824.png)

读离客户端最近的DN上的数据

![image-20201004210849493](C:\Users\liuwe\AppData\Roaming\Typora\typora-user-images\image-20201004210849493.png)

下载一个文件的全部是全集操作，那么子集操作显然也是可以的，即不下载文件的全部



回到最开始的问题：为什么要用HDFS？

并行计算的核心



### MapReduce

![image-20201006113312216](C:\Users\liuwe\AppData\Roaming\Typora\typora-user-images\image-20201006113312216.png)

在HDFS中，文件已经被切为块了，这是一种物理层面的切割，那么为什么我们还需要把这些物理块包装为切片传递给map呢？这在软件工程学上称之为解耦合，因为我们的一次任务可能分为两个部分，一部分是IO密集型计算，另一部分是CPU密集型计算，假设我们的IO计算比CPU计算慢得多，那么我们在将块转换为切片时就需要为IO计算分配的切片小一点，而为CPU计算分配的切片大一些

总而言之——控制粒度，增加并行度



切片可以复用块的信息，切片知道每一个块在集群中的哪些机器上，那么就可以把map逻辑动态分配到这些计算机上去，这叫做计算向数据移动。



map的并行度由切片的数量决定

reduce的并行度由人来决定，和机器数量，key的数量有关系

框架当中默认切片数量就是块的数量，reduce的数量是1个，所以需要人手动调整



MapReduce：

数据以一条记录为单位经过map方法映射成KV键值对，相同的key为一组，这一组数据调用一次reduce方法，在方法内迭代计算这一组数据

什么是迭代器模式？

数据集一般是用迭代计算的方式

reduce从map中拉过来的组可能会特别大，比如说几个T，这样整个内存是放不下这么大的数据的，但MR框架仍然可以成功完成任务，这是因为传递给reducer的并不是数据集本身，而是这个数据集的迭代器，一个Object，迭代器迭代的时候数据不在内存，在磁盘。等于说你对着文件迭代的时候，打开一个IO，包装成一个迭代器，有一条读一条。

IO成本是多少？



![image-20201007104731427](C:\Users\liuwe\AppData\Roaming\Typora\typora-user-images\image-20201007104731427.png)

### Combiner

combiner在本地先做一次小的reduce，以减少网络带宽的压力



### Partitioner

可以自定义Partitioner，一般用哈希来实现，使得相同的key被映射到同一reducer上



#### 负载均衡

S打头的单词多，Z打头的单词少，成为性能瓶颈

设置Patitioner使得ReduceTask均衡