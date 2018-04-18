---
title: "常见问题(FAQ)"
---
<!--
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->

以下是关于Flink项目的常见问题。如果你还有其他问题，可以[参阅文档](http://flink-china.org/faq.html)或询问[社区](http://flink-china.org/faq.html)。

{% toc %}





## 综述

### Flink是一个Hadoop项目吗？

Flink是一个数据处理系统，是**Hadoop的MapReduce组件的替代品**。它有自己运行环境，而不是建立在MapReduce上。因此，它可以完全独立于Hadoop生态系统。尽管如此，Flink依然可以接入Hadoop的分布式文件系统(HDFS)来读取和写入数据，也可以使用Hadoop下一代资源调度系统（YARN）提供集群资源调度。由于绝大多数Flink用户都使用Hadoop HDFS来存储数据，Flink已经将所需的库接入到HDFS上。

### 我需要安装Apache Hadoop来使用Flink吗?

**不是**。Flink可以在**没有**Hadoop的环境下运行。然而，通常的设置是使用Flink分析存储在Hadoop Distributed File System（HDFS）上的数据。为了使这些设置能够正常工作，Flink在默认情况下集成了Hadoop客户端。
此外，我们为现有的Hadoop YARN集群提供了特别的YARN Flink下载版本。 [Apache Hadoop
YARN](http://hadoop.apache.org/docs/r2.2.0/hadoop-yarn/hadoop-yarn-site/YARN.html)
是Hadoop的集群资源管理器，它允许在集群上使用不同的执行引擎。

## 用法

### 如何评估Flink计划的进展？

有多种方法可以跟踪Flink计划的进展情况：

- JobManager（分布式系统的主设备）启动Web界面以观察程序执行。 在默认情况下在端口8081上运行（在conf / flink-config.yml中配置）。
- 从命令行启动程序时，随着程序在操作中的进展，它将打印所有操作员的状态更改。
- 所有状态更改也会记录到JobManager的日志文件中。

### 怎样才能找出程序失败的原因？

- JobManager Web前端（默认情况下在端口8081上）显示失败任务的例外情况。
- 如果从命令行运行程序，任务例外将打印到标准错误流并显示在控制台上。
- 通过命令行和Web界面，您可以确定哪个并行任务首先失败，并导致其他任务取消执行。
- 发生异常的主服务器和工作服务器的日志文件(`log/flink-<user>-jobmanager-<host>.log` 和`log/flink-<user>-taskmanager-<host>.log`).

### 如何调试Flink程序？

- 当您使用[LocalExecutor]({{site.docs-snapshot}}/apis/local_execution.html)在本地启动程序时，可以在函数中放置断点并像普通的Java / Scala程序一样调试它们。
- [Accumulators]({{ site.docs-snapshot }}/apis/programming_guide.html#accumulators--counters) 在跟踪并行执行的行为方面非常有用。它们允许您收集程序操作中的信息并在程序执行后显示它们。

### 平行度是什么？如何设置它？

在Flink程序中，并行性决定了操作如何分解为分配给任务槽的单个任务。 群集中的每个节点至少有一个任务槽。 任务槽的总数是所有机器上所有任务槽的数量。 如果并行性设置为`N`, 则Flink会尝试将操作划分为 `N` 个并行任务，这些任务可以使用可用任务槽同时计算。 任务槽的数量应该等于并行度，以确保所有任务可以同时在一个任务槽中计算。

注意: 并非所有操作都可以分为多个任务。 例如，没有分组的
`GroupReduce` 操作必须以1的并行性执行，因为整个组需要存在于恰好一个节点以执行减少操作。link将确定并行性是否为1并相应地设置它。
并行性可以通过多种方式进行设置，以确保对Flink程序的执行进行精细控制。 有关如何设置并行性的详细说明，请参阅查阅 [配置指南]({{ site.docs-snapshot }}/setup/config.html#common-options) 。 另外请查看 [this figure]({{ site.docs-snapshot }}/setup/config.html#configuring-taskmanager-processing-slots) 详细说明处理插槽和并行性是如何相互关联的。

## 错误

### 为什么我会收到“NonSerializableException”？

Flink中的所有函数必须是序列化的，这是由 [java.io.Serializable](http://docs.oracle.com/javase/7/docs/api/java/io/Serializable.html)定义的。.
由于所有的程序接口都是可序列化的，异常意味着您的函数中使用的一个域不是可序列化的。


特别地，如果你的函数是一个内部类，或者匿名内部类，它包含一个隐藏的对封闭类的引用。（通常叫做this$0，如果你看一下调试器中的函数）如果封闭类不是可序列化的，   那么这可能就是错误的根源。解决方案：

- 使该函数成为一个独立的类，或者一个静态内部类（不再引用封闭类）
- 使封闭类可串行化
- 使用Java 8 lambda函数

### 在Scala API中，我收到了关于隐式值和证据参数的报错

这意味着不能提供类型信息的隐式值。确保在你的代码中有 `import org.apache.flink.api.scala._` 的声明.

如果你在使用通用参数的函数或类中使用Flink操作，那么必须为该参数提供类型定义。可以通过上下文邦定来实现：

~~~scala
def myFunction[T: TypeInformation](input: DataSet[T]): DataSet[Seq[T]] = {
  input.reduceGroup( i => i.toSeq )
}
~~~

参阅 [类型提取和序列化]({{ site.docs-snapshot }}/internals/types_serialization.html) 了解关于Flink如何处理类型的深入讨论。

### 我收到一条错误消息，说没有足够的缓冲区可用。我怎么解决这个问题？

 如果您在一个大规模并发环境（100多个并行线程）中运行Flink，您需要通过配置参数
`taskmanager.network.numberOfBuffers`.
调整网络缓冲区的数量。根据经验法则，缓冲区的数量应该是最少的
`4 * numberOfTaskManagers * numberOfSlotsPerTaskManager^2`. 查阅
[Configuration Reference]({{ site.docs-snapshot }}/setup/config.html#configuring-the-network-buffers) 获取更多细节.

### 我的作业在早期就因java.io.EOF异常失败。可能的原因是什么？

对于这些异常，最常见的情况是当Flink设置时对应错了HDFS的版本。不同版本的HDFS经常不兼容，导致文件系统主服务器和客户机之间的连接中断。


~~~bash
Call to <host:port> failed on local exception: java.io.EOFException
    at org.apache.hadoop.ipc.Client.wrapException(Client.java:775)
    at org.apache.hadoop.ipc.Client.call(Client.java:743)
    at org.apache.hadoop.ipc.RPC$Invoker.invoke(RPC.java:220)
    at $Proxy0.getProtocolVersion(Unknown Source)
    at org.apache.hadoop.ipc.RPC.getProxy(RPC.java:359)
    at org.apache.hadoop.hdfs.DFSClient.createRPCNamenode(DFSClient.java:106)
    at org.apache.hadoop.hdfs.DFSClient.<init>(DFSClient.java:207)
    at org.apache.hadoop.hdfs.DFSClient.<init>(DFSClient.java:170)
    at org.apache.hadoop.hdfs.DistributedFileSystem.initialize(DistributedFileSystem.java:82)
    at org.apache.flinkruntime.fs.hdfs.DistributedFileSystem.initialize(DistributedFileSystem.java:276
~~~

有关如何为不同的Hadoop和HDFS版本设置Flink的详细信息，请参考 [下载页面]({{ site.baseurl }}/downloads.html#maven) 和 {% github README.md master "构建说明" %}

### 我的作业因为HDFS/Hadoop代码的各种异常失败。我能做什么?

默认情况下，Flink将附带Hadoop 2.2版本的二进制文件。这些二进制文件用于连接HDFS或YARN。HDFS客户端似乎有一些bug，在写入HDFS时（特别是在高负载下）会导致异常。以下是异常的情况：


- `HDFS client trying to connect to the standby Namenode "org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.ipc.StandbyException): Operation category READ is not supported in state standby"`
- `java.io.IOException: Bad response ERROR for block BP-1335380477-172.22.5.37-1424696786673:blk_1107843111_34301064 from datanode 172.22.5.81:50010
  at org.apache.hadoop.hdfs.DFSOutputStream$DataStreamer$ResponseProcessor.run(DFSOutputStream.java:732)`

- `Caused by: org.apache.hadoop.ipc.RemoteException(java.lang.ArrayIndexOutOfBoundsException): 0
        at org.apache.hadoop.hdfs.server.blockmanagement.DatanodeManager.getDatanodeStorageInfos(DatanodeManager.java:478)
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.updatePipelineInternal(FSNamesystem.java:6039)
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.updatePipeline(FSNamesystem.java:6002)`

如果您经历了以上任何一种，我们建议使用一个与您的本地HDFS版本相匹配的Hadoop版本的Flink构建。您还可以根据Hadoop版本（例如使用自定义补丁级别的Hadoop）手动构建Flink。


### Eclipse开发中，Scala项目中的编译错误

Flink使用了Scala编译器的一个新特性（称为“准引号”），它还没有与Eclipse Scala插件进行很好的集成。为了使这个特性在Eclipse中可用，您需要手动配置Flink-Scala项目来使用编译器插件：


- 右键点击“Flink-Scala”并选择“属性”；
- 选择 "Scala Compiler" 并点击 "Advanced" 选项卡. 如果没有，您可能没有为Scala设置合适的Eclipse）
- 检查框“"Use Project Settings"
- 在 "Xplugin"字段中, 将路径 "/home/<user-name>/.m2/repository/org/scalamacros/paradise_2.10.4/2.0.1/paradise_2.10.4-2.0.1.jar"
  注意：您必须首先在命令行上与Maven构建Flink，以确保插件被下载。

### 我的程序没有计算出正确的结果。为什么我自定义keys没有被正确地grouped/joined？


  Keys必须正确地实现 `java.lang.Object#hashCode()`,
`java.lang.Object#equals(Object o)`, 和 `java.util.Comparable#compareTo(...)`.
这些方法总是支持默认的实现，而这些实现通常是不充分的。因此，所有的键必须覆盖`hashCode()`和`equals(Object o)`。

### 我的数据类型发生java.lang.Instantiation异常，哪里出错了？

所有的数据类型类都必须是公有的，并且有一个公共的nullary构造函数（没有参数的构造函数）。此外，类不能是抽象的或接口。如果类是内部类，它们必须是公有的和静态的。


### 我用停止脚本停不掉Flink，怎么办？

停止进程有时需要几秒钟，因为停止可能会进行一些清理工作。
在一些错误情况下用提供的停止脚本(`bin/stop-local.sh` or `bin/stop-
cluster.sh`)停不掉作业管理器或者任务管理器。你可以在linux/mac上kill它们的进程：

- 确定作业管理器或者任务管理器进程的进程id（pid），你可以在Linux上用 `jps`命令（如果OpenSDK已经安装）或者`ps -ef | grep java`命令找到所有的java进程。

- 用`kill -9 <pid>`命令结束掉受影响的JobManager或者TaskManager的`pid`。
在Windows系统上，任务管理器列出了所有进程的表，并允许您通过他的入口来销毁进程。

在Windows系统上，任务管理器列出了所有进程的表，并允许您通过他的入口来销毁进程。

作业管理器和任务管理器服务都将把信号（如SIGKILL和SIGTERM）写入各自的日志文件中。这对于调试停止问题很有帮助

### 我收到了OutOfMemory异常，该怎么处理？

这些异常通常发生在程序中的函数消耗大量内存来收集对象，例如列表或图表。Java中的OutOfMemory异常有点棘手。这个异常并不一定是由分配了太多内存组件引起，而是由组件试图响应的最新的内存无法得到释放。

有两种解决方法：

1. 您是否可以在函数中使用更少的内存。例如，使用原始类型的数组而不是对象类型。

2. 减少Flink为其自身处理而保留的内存。任务管理器保留了可用内存的一部分，用于排序、散列、缓存、网络缓冲等。用户定义函数无法访问该部分内存。通过保留它，系统可以保证在大量输入不会耗尽内存，但是如果需要，可以使用可用内存和destage操作来进行划分。默认情况下，系统会保留大约70%的内存。如果您经常在udf中运行需要更多内存的应用程序，您可以使用`taskmanager.memory.fraction` 或者`taskmanager.memory.size`来减少该值。参阅[Configuration Reference]({{ site.docs-snapshot }}/setup/config.html)获取更多细节。这将为JVM堆留下更多内存，但可能会导致数据处理任务更频繁地进入磁盘。

 OutOfMemoryExceptions的另一个原因是使用了错误的状态后端。默认情况下，Flink在流作业中使用基于堆的状态后端。RocksDBStateBackend允许状态尺寸大于可获得的堆空间。

### 任务管理器的日志文件为什么变得如此巨大？

检查您的作业的日志记录行为。每个对象或元组的日志记录可能有助于在小的数据集安装中来调试作业，但是如果用于大的输入数据会限制性能并消耗大量的磁盘空间。
分配给我的任务管理器的槽已经被释放了。我应该做什么?

### 分配给我的任务管理器的槽已经被释放了。我应该做什么?


如果你看到java.lang.Exception: The slot in which the task was executed has been released. Probably loss of TaskManager 异常，即使任务管理器没有崩溃，它意味着任务管理器已经有一段时间没有响应。这可能是由于网络问题造成的，但通常是由于长期的垃圾收集摊位。在这种情况下，快速解决方案将是使用增量垃圾收集器，如G1垃圾收集器。它通常会带来更短的暂停。此外，您可以通过减少对其内部操作的内存占用（参见任务管理器托管内存的配置）来为用户代码提供更多的内存。

如果这两种方法都失败了，并且错误仍然存在，只需通过设置AKKA_WATCH_HEARTBEAT_PAUSE (akka.watch.heartbeat.pause)到更大的值（例如600秒），就可以增加TaskManager的脉搏暂停。这将导致作业管理器在考虑丢失之前等待一个更长的时间间隔。

## YARN开发

### YARN会话只持续了数秒

./bin/yarn-session.sh脚本在YARN会话开始时运行。在一些错误情况下，脚本立即停止执行。类似结果如下：

~~~
07:34:27,004 INFO  org.apache.hadoop.yarn.client.api.impl.YarnClientImpl         - Submitted application application_1395604279745_273123 to ResourceManager at jobtracker-host
Flink JobManager is now running on worker1:6123
JobManager Web Interface: http://jobtracker-host:54311/proxy/application_1295604279745_273123/
07:34:51,528 INFO  org.apache.flinkyarn.Client                                   - Application application_1295604279745_273123 finished with state FINISHED at 1398152089553
07:34:51,529 INFO  org.apache.flinkyarn.Client                                   - Killing the Flink-YARN application.
07:34:51,529 INFO  org.apache.hadoop.yarn.client.api.impl.YarnClientImpl         - Killing application application_1295604279745_273123
07:34:51,534 INFO  org.apache.flinkyarn.Client                                   - Deleting files in hdfs://user/marcus/.flink/application_1295604279745_273123
07:34:51,559 INFO  org.apache.flinkyarn.Client                                   - YARN Client is shutting down
~~~

这里的问题是ApplicationMaster（AM）停止了，而YARN客户端却认为AM已经完成。

这种情况有三种可能的原因：

- AM由于异常而退出。调试异常要查看容器的日志文件。yarn-site.xml文件包含已配置的路径。路径的key值为yarn.nodemanager.log-dirs，缺省值是${yarn.log.dir}/userlogs

- YARN已经结束了AM的容器。当AM使用过多的内存或其他资源超出了YARN的限制时，就会发生这种情况。在这种情况下，主机上的节点管理日志中会保存错误信息。

- T	操作系统关闭AM的JVM。如果YARN配置错误并且配置超过物理内存，就会发生这种情况。在AM运行的机器上执行dmesg可以查看是否发生这种情况。您可以从Linux的[OOM killer](http://linux-mm.org/OOM_Killer).

###  YARN容器由于消耗太多内存而被kill

通常会显示类似下面的一个log信息：

~~~
Container container_e05_1467433388200_0136_01_000002 is completed with diagnostics: Container [pid=5832,containerID=container_e05_1467433388200_0136_01_000002] is running beyond physical memory limits. Current usage: 2.3 GB of 2 GB physical memory used; 6.1 GB of 4.2 GB virtual memory used. Killing container.
~~~

这种情况下，JVM进程太大。由于Java堆大小固定，额外内存来自于非堆源：

  - 使用非堆内存的库。（Flink自身的堆外内存是有限的，并在计算堆允许大小时考虑在内）
  - PermGen空间（字符串和类）、代码缓存、内存映射jar文件。
  - 本地库(RocksDB)

You can activate the [memory debug logger](https://ci.apache.org/projects/flink/flink-docs-release-1.0/setup/config.html#memory-and-performance-debugging) to get more insight into what memory pool is actually using up too much memory.


### 启动过程中HDFS许可异常引起的YARN会话崩溃

在开始YARN会话时，您会收到这样的异常：

~~~
Exception in thread "main" org.apache.hadoop.security.AccessControlException: Permission denied: user=robert, access=WRITE, inode="/user/robert":hdfs:supergroup:drwxr-xr-x
  at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:234)
  at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:214)
  at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkPermission(FSPermissionChecker.java:158)
  at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkPermission(FSNamesystem.java:5193)
  at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkPermission(FSNamesystem.java:5175)
  at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkAncestorAccess(FSNamesystem.java:5149)
  at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.startFileInternal(FSNamesystem.java:2090)
  at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.startFileInt(FSNamesystem.java:2043)
  at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.startFile(FSNamesystem.java:1996)
  at org.apache.hadoop.hdfs.server.namenode.NameNodeRpcServer.create(NameNodeRpcServer.java:491)
  at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.create(ClientNamenodeProtocolServerSideTranslatorPB.java:301)
  at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java:59570)
  at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:585)
  at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:928)
  at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2053)
  at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2049)
  at java.security.AccessController.doPrivileged(Native Method)
  at javax.security.auth.Subject.doAs(Subject.java:396)
  at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1491)
  at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2047)

  at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
  at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:39)
  at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:27)
  at java.lang.reflect.Constructor.newInstance(Constructor.java:513)
  at org.apache.hadoop.ipc.RemoteException.instantiateException(RemoteException.java:106)
  at org.apache.hadoop.ipc.RemoteException.unwrapRemoteException(RemoteException.java:73)
  at org.apache.hadoop.hdfs.DFSOutputStream.newStreamForCreate(DFSOutputStream.java:1393)
  at org.apache.hadoop.hdfs.DFSClient.create(DFSClient.java:1382)
  at org.apache.hadoop.hdfs.DFSClient.create(DFSClient.java:1307)
  at org.apache.hadoop.hdfs.DistributedFileSystem$6.doCall(DistributedFileSystem.java:384)
  at org.apache.hadoop.hdfs.DistributedFileSystem$6.doCall(DistributedFileSystem.java:380)
  at org.apache.hadoop.fs.FileSystemLinkResolver.resolve(FileSystemLinkResolver.java:81)
  at org.apache.hadoop.hdfs.DistributedFileSystem.create(DistributedFileSystem.java:380)
  at org.apache.hadoop.hdfs.DistributedFileSystem.create(DistributedFileSystem.java:324)
  at org.apache.hadoop.fs.FileSystem.create(FileSystem.java:905)
  at org.apache.hadoop.fs.FileSystem.create(FileSystem.java:886)
  at org.apache.hadoop.fs.FileSystem.create(FileSystem.java:783)
  at org.apache.hadoop.fs.FileUtil.copy(FileUtil.java:365)
  at org.apache.hadoop.fs.FileUtil.copy(FileUtil.java:338)
  at org.apache.hadoop.fs.FileSystem.copyFromLocalFile(FileSystem.java:2021)
  at org.apache.hadoop.fs.FileSystem.copyFromLocalFile(FileSystem.java:1989)
  at org.apache.hadoop.fs.FileSystem.copyFromLocalFile(FileSystem.java:1954)
  at org.apache.flinkyarn.Utils.setupLocalResource(Utils.java:176)
  at org.apache.flinkyarn.Client.run(Client.java:362)
  at org.apache.flinkyarn.Client.main(Client.java:568)
~~~
出现这种错误的原因是，**in HDFS**中用户的主目录权限错误。用户(这个例子中的`robert`）不能在主目录下创建子目录。

Flink在用户主目录中创建一个`.flink/`目录，其中存储Flink jar和配置文件。


### 我的作业取消为什么没有反应?

Flink通过在所有用户任务上调用 `cancel()`方法来取消作业。理想情况下，任务对调用进行正确的响应，停止当前运行，这样所有线程都将关闭。

如果任务在一定的时间内没有反应，Flink将会周期性地中断线程。

TaskManager日志还包含用户代码被阻塞时调用方法的堆栈。

## 产品特点

### Flink提供什么样的容错系统?

对于流式计算来说，Flink采用新方法来绘制流数据状态的周期性快照，并使用它们进行恢复。这种机制高效灵活。 参阅 [streaming fault tolerance]({{ site.docs-snapshot }}/internals/stream_checkpointing.html) 获得更多细节。

对于批量计算，Flink会记住程序的转换序列，并可以重启失败的作业。

### Flink是否支持Hadoop式计数器和分布式缓存？

[Flink的存储池]({{ site.docs-snapshot }}/apis/programming_guide.html#accumulators--counters) 和
Hadoop的计算器相比工作很类似, 但比后者更为强大.

Flink 的[分布式缓存](https://github.com/apache/flink/tree/master/flink-core/src/main/java/org/apache/flink/api/common/cache/DistributedCache.java) 可以与API深度集成。 请查阅 [JavaDocs](https://github.com/apache/flink/tree/master/flink-java/src/main/java/org/apache/flink/api/java/ExecutionEnvironment.java#L831) 获取更多关于应用的细节来了解如何使用它。

为了使数据集在所有任务上可用，我们鼓励您使用 [Broadcast Variables]({{ site.docs-snapshot }}/apis/programming_guide.html#broadcast-variables) instead. 它们比分布式缓存更高效、简单。
