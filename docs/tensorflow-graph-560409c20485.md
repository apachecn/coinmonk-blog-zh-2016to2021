# 张量流图

> 原文：<https://medium.com/coinmonks/tensorflow-graph-560409c20485?source=collection_archive---------8----------------------->

这篇帖子讲的是 tensorflow 如何执行你的机器学习模型。我们将简要概述张量流图的组成部分，然后深入研究如何在单个和多个设备上执行该图。

张量流图具有以下属性。每个**节点**有**零个或多个输入**，代表一个操作的**实例化。**

从图的边缘流出的值被称为**张量**。这些张量在经过这些**节点**时会经历各种变换。

张量是**任意维度数组**，其中**的底层元素类型**是在图形构建时推断出来的。这使得 Tensorflow 非常快，因为它通过这个图知道未来会发生什么操作。因此，这些知识允许各种编译时优化。

特殊边被称为控制依赖——没有数据流经这些边，但是它们表示控制依赖的**源节点**必须在**目的节点能够执行**之前完成执行

这个属性允许客户端在关系之前执行**关系。例如，这对于控制峰值内存使用非常有帮助。**

# 操作和内核

一个操作定义一个计算:例子可以是

1.  增加
2.  矩阵乘法

一个操作可以有**属性**。属性的一个用例是使操作**多态**(在相同数据类型的元素之间执行操作)

内核被定义为:一个操作的**实现，它可以在**特定类型的设备** (CPU、GPU、TPU)等上运行。**

# 会议

客户端通过创建一个**会话**与 Tensorflow 系统进行交互。

1.  session 接口有一个方法叫做**扩展**。这允许我们用额外的**节点和边**修改计算图。
2.  会话接口有另一个方法**运行**。

*   该函数计算所有节点的**传递闭包，为了计算请求的输出，必须执行这些节点。**
*   然后**按照尊重其依赖性的顺序**排列节点

通常，张量流的用途是

1.  设置一次带有图形的会话。
2.  通过*运行*运行图形或不同的子图形数千次或数百万次

**注**:图的传递闭包是定义图中每个节点之间可达性的矩阵。这个矩阵将用 0 和 1 填充。 **0** 定义不可达， **1** 定义可达

# 变量

一个变量是一个**持久张量**。大多数张量在运行操作后无法存活。运行操作后，变量**继续存在。变量的用例在**中存储神经网络**的参数。当图形上的*运行*被调用时，这些变量被更新。**

# 设备

**工人操作一个或多个设备**。这些设备可以是 CPU 内核、GPU 等。它们由设备名称和设备类型来标识。设备名称的 Eg 可以是

`/job:localhost/device:cpu:0`

在分布式设置中，作业名称是指设备正在执行的作业。每个设备对象有两个功能:

1.  **分配/解除分配**内存
2.  **安排更高层请求的内核**的执行。

# 张量

**类型的多维数组**，这些张量是 Tensorflow 的基础数据类型。张量可以有各种类型，从:

1.  8 位至 64 位
2.  IEEE 浮点型和双精度型
3.  复数数据类型
4.  字符串类型(任意字节数组)

# 执行图表:实现视角

# 概观

客户端 Tensorflow 中的主实体。客户端联系主进程和一个或多个工作进程。

工作进程处理与 GPU 或 CPU 内核等设备的计算。

张量流中有两种设置:

1.  **本地设置** —客户端、主机和工人都在**同一台电脑中。**
2.  **分布式设置** —客户端、主机和工人都可以在**不同的设备**中。在分布式设置中，我们在容器中运行这些不同的组件。这些容器通过像 Kubernetes 这样的集群调度系统进行调度。

# 单一设备设置

运行 Tensorflow 最简单的场景。

1.  单个工作进程
2.  单一设备

节点的处理方式尊重节点之间的依赖关系。更具体地说

*   每个节点保持有多少依赖节点需要被处理的计数*。每当执行一个依赖项时，该计数就递减。*
*   当*计数*为 **0** 时，该节点被放入就绪队列，进行后续处理。

请注意:*未指定就绪队列如何处理节点*

# 多设备设置

一旦我们有了多种设备。我们有两件事要担心:

1.  决定将每个节点的计算放在哪个设备上
2.  管理这些设备之间的通信。

# 节点布局

节点放置算法计算出**将什么节点给予什么设备**。该算法使用一个**成本模型**来做出决策。根据白皮书，节点放置算法使用**贪婪试探法**，通过成本模型和其他参数来决定将节点放置在哪个设备中。这种贪婪的试探法考虑到了

1.  执行计算的**成本。**
2.  **从其他设备向该节点**传送输入的成本。

该计算将**最快完成的器件**被选为器件。这个放置过程一直持续到节点被放置。

这个算法现在可能已经改变了，因为这篇论文是在 2016 年写的。

一旦在设备中放置了节点，就需要在这些设备之间放置通信协议。

# 设备间通信

Tensorflow **移除不同设备中节点**之间的边，而**用发送和接收调用**来替换它们。在运行时，发送和接收调用协调在设备间传输数据。

这种方法有以下好处:

1.  数据仅通过接收调用**发送一次**,内存仅针对单个张量分配一次。因此张量的所有用户将不需要他们单独的接收/发送呼叫。
2.  通过以这种方式处理通信，我们让设备中不同节点的调度**分散到工人**中。主设备不需要跟踪这一点，因为*发送*和*接收*调用处理不同工人和设备之间的同步。

# 分布式环境中的执行

分布式设置与多设备设置非常相似。因为发送和接收调用是通过 **TCP 或 RDMA** 调用来跨机器边界移动数据的。分布式设置中的执行需要容错。故障通过两种方式检测:

1.  **发送和接收呼叫之间的通信**出错。
2.  从主流程到每个工人流程的定期健康检查。

当检测到故障时，整个图形执行被**中止并从零开始。**

然而，tensorflow 系统支持**检查点和重启后的恢复**。

变量值通过所谓的**保存节点**进行检查

这些保存节点与变量相连接。这些节点可以配置为定期执行。比如每 N 次迭代后，或者从不超过 N 秒后一次。

类似地，这些变量也与一个**恢复节点**连接，这样它们的值在重启后被恢复。

# 在多种设备上进行训练的技术

# 同步 SGD

这个 SGD 依赖于一个跟踪模型参数的主服务器和几个执行一些计算的工作线程。这些工人然后将数据发送回主设备以更新参数。一旦 master 接收到来自工人的所有参数，它就会累积这些梯度，然后向每个工人发送新梯度的副本，以便工人可以处理下一批

# 异步 SGD

以上方法论不错，但我们可以做得更好。异步 SGD 仅仅意味着主设备在接收到一些参数后，执行更新并将梯度推送到所有的工作设备。它不会等到所有的工人都完成任务。

# 模型并行训练

用于训练深层 LSTMS。这种类型的训练是模型计算的不同部分在不同的计算设备上对同一批例子同时进行**。**

# 模型计算流水线的并发步骤

另一种更好地利用训练深度神经网络的常见方法是在同一设备中流水线化模型的计算。它是与 ASGD 完全相同的**，但不是多个设备，而是在同一个设备**中执行相同的模型，以更好地利用设备能力来并行操作。

# 结论

总之，Tensorflow 是一个支持以下功能的系统

1.  **在多个设备上进行训练和推理**，非常适合在**分布式设置中使用**。
2.  通过**数据流图结构，其设计方式有利于未来的优化。**
3.  通过使用**压缩技术，使设备之间的通信更加简单。**
4.  **布局算法**特别有趣，作者说它有可能被深度学习算法取代**。**