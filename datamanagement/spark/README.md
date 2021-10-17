# Apache Spark

### Introducton

spark是一个实现快速通用的集群计算平台。它是由加州大学伯克利分校AMP实验室 开发的通用内存并行计算框架，用来构建大型的、低延迟的数据分析应用程序。它扩展了广泛使用的MapReduce计算 模型。高效的支撑更多计算模式，包括交互式查询和流处理。**spark的一个主要特点是能够在内存中进行计算，及时依赖磁盘进行复杂的运算**.

Spark提供了统一的解决方案。Spark可以用于批处理、交互式查询（Spark SQL）、实时流处理（Spark Streaming）、机器学习（Spark MLlib）和图计算（GraphX）。这些不同类型的处理都可以在同一个应用中无缝使用。Spark统一的解决方案非常具有吸引力，毕竟任何公司都想用统一的平台去处理遇到的问题，减少开发和维护的人力成本和部署平台的物力成本。

In summary, Spark is a platform providing access to clusters, data sources, and libraries for large-scale computing

> 总而言之，Spark是一个平台，可提供对集群，数据源和库的访问以进行大规模计算

![ Spark as a unified analytics engine for large-scale data processing](<../../.gitbook/assets/image (35).png>)

Describing Spark as large scale implies that a good use case for Spark is tackling problems that can be solved with multiple machines. For instance, when data does not fit on a single disk drive or into memory, Spark is a good candidate to consider.

> 大规模描述Spark意味着Spark的一个很好的用例是解决可以用多台机器解决的问题。例如，当数据不能放在单个磁盘驱动器或内存中时，Spark是一个不错的选择。

You can also consider it for problems that might not be large scale, but for which using multiple computers could speed up computation. For instance, CPU-intensive models and scientific simulations also benefit from running in Spark.

> 您也可以考虑将其用于可能不是大规模的问题，但使用多台计算机可以加快计算速度。例如，CPU密集型模型和科学仿真也可以从在Spark中运行而受益。

Therefore, Spark is good at tackling large-scale data-processing problems, usually known as big data (datasets that are more voluminous and complex than traditional ones) but it is also good at tackling large-scale computation problems, known as big compute (tools and approaches using a large amount of CPU and memory resources in a coordinated way).  

> 因此，Spark擅长解决通常称为大数据的大规模数据处理问题（比传统数据集更为庞大和复杂的数据集），但也擅长解决称为大计算的大规模计算问题（工具和方法以协调的方式使用大量CPU和内存资源）。 

### Spark组成

Spark组成(BDAS)：全称伯克利数据分析栈，通过大规模集成算法、机器、人之间展现大数据应用的一个平台。也是处理大数据、云计算、通信的技术解决方案。它的主要组件有：

* **SparkCore**：将分布式数据抽象为弹性分布式数据集（RDD），实现了应用任务调度、RPC、序列化和压缩，并为运行在其上的上层组件提供API。
* **SparkSQL**：Spark Sql 是Spark来操作结构化数据的程序包，可以让我使用SQL语句的方式来查询数据，Spark支持 多种数据源，包含Hive表，parquest以及JSON等内容。
* **SparkStreaming**： 是Spark提供的实时数据进行流式计算的组件。
* **MLlib**：提供常用机器学习算法的实现库。
* **GraphX**：提供一个分布式图计算框架，能高效进行图计算。
* **BlinkDB**：用于在海量数据上进行交互式SQL的近似查询引擎。
* **Tachyon**：以内存为中心高容错的的分布式文件系统。

###  **Spark基本概念**

* RDD：是弹性分布式数据集（Resilient Distributed Dataset）的简称，是分布式内存的一个抽象概念，提供了一种高度受限的共享内存模型。
* DAG：是Directed Acyclic Graph（有向无环图）的简称，反映RDD之间的依赖关系。
* Driver Program：控制程序，负责为Application构建DAG图。
* Cluster Manager：集群资源管理中心，负责分配计算资源。
* Worker Node：工作节点，负责完成具体计算。
* Executor：是运行在工作节点（Worker Node）上的一个进程，负责运行Task，并为应用程序存储数据。
* Application：用户编写的Spark应用程序，一个Application包含多个Job。
* Job：作业，一个Job包含多个RDD及作用于相应RDD上的各种操作。
* Stage：阶段，是作业的基本调度单位，一个作业会分为多组任务，每组任务被称为“阶段”。
* Task：任务，运行在Executor上的工作单元，是Executor中的一个线程。(Application由多个Job组成，Job由多个Stage组成，Stage由多个Task组成。Stage是作业调度的基本单位。)
