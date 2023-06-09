# 扩展区块链与扩展纠纷

> 原文：<https://medium.com/coinmonks/scaling-a-blockchain-vs-scaling-a-tangle-8b7182eda980?source=collection_archive---------0----------------------->

![](img/fd8860eb0180f77abf2430b98458839d.png)

## 指出 DAG 可扩展性中可能存在的弱点

从表面上看，IOTA 和 Byteball 等分布式分类帐使用的 DAG(有向无环图)设计看起来像是一项惊人的创新。代替在现有区块链设计中用作事务容器的大块，DAG 构建引用较旧事务的事务图，因此当节点接收到事务时可以立即确认事务，而不必等待下一个块。

任何试图使用区块链的人都会很快意识到，快速的交易确认时间是一种优势，而不是必须等待交易被分组到一个块中才能依赖于它们的状态或余额变化。

事实上，只要 DAG 节点已经知道由该事务批准的两个事务，DAG 就快速确认事务。但是同步节点之间的状态似乎是现有 DAG 实现的主要问题，例如，IOTA 当前依赖于单个协调器节点，而 Byteball 依赖 12 个见证节点，所有这些见证节点都由开发者自己控制，以检查 DAG 的状态。

但是为什么 DAG 的同步比区块链的同步更困难呢？简而言之，因为区块链状态被每个数据块修改，而 DAG 状态被每个事务修改。

作为一个在区块链负载测试和扩展上花费了大量时间的人，我可以证明，在重负载下运行的分散式网络会自然地产生分叉，即使所有参与者都诚实并按照协议规则工作。我还可以证明，切换到更好的分支是节点需要执行的最繁重的操作之一，因为它必须撤销弹出块中现有事务所做的更改，并应用更好分支的块中包含的事务所做的更改。

然而，我认为，区块链优于 DAG 的一个优点是，区块链只对块的顺序敏感，而对事务的顺序不敏感，这一事实使得它的可伸缩性不亚于 DAG。

正如我在上一篇关于[扩展](/@lyaffe/visa-scale-blockchain-565423c54fd8)的帖子中解释的那样，在分布式网络中，不同的节点将在不同的时间以不同的顺序看到事务，事务的速率越高，这些排序差异出现的频率就越高。

对于区块链来说，这不是一个严重的问题，因为块生成被人为地变得困难，从而将以太坊上的平均块生成频率限制为 15 秒，NXT/Ardor 上为 1 分钟，比特币上为 10 分钟。这意味着，即使在重负载下，由冲突块引起的分叉也很少发生，并且当它们确实发生时，节点有时间执行切换到更好的分叉所需的大量处理，以便在生成更多块之前与另一个节点同步状态。

然而，在 DAG 实现中，事务传播的等待时间和可能的顺序差异导致了几个问题，同时事务吞吐量增加，并且节点开始获得由其他节点提交的事务，对于这些事务，一个或两个被批准的事务对它们来说还不知道，因此将不能将它们添加到 DAG。

DAG 的拥护者声称，为了消除协调器和白色节点的要求，您必须有大量的并发事务，作为回应，我想指出 DAG 实现中的几个弱点，我预测这些弱点将出现在每秒接受 1000 个并发事务甚至更少的 DAG 网络上。

在 1000 TPS 的网络中，当远程 DAG 节点接收事务时，比方说，在事务被提交后的一秒钟(回想一下，光速是最终的，并且互联网的运行速度比光速慢得多)，该远程节点已经落后于网络中其他更中心的节点 1000 个事务，这将导致几个问题

(1)该节点几乎肯定不能立即处理一些事务，因为提交者节点看到的未批准的事务仍然通过网络传播，因此必须将它们保存在可能不断增长的未确认池中，这将耗尽节点资源。

(2)当最终一个事务的子纠结的所有被批准的祖先将到达该节点时，这些事务可以批准已经被隐藏在 DAG 的当前 tip 之后的许多嵌套层的事务，并且当负载到达某个临界点时，DAG 将开始在所有方向上的云状扩展，其中不断增长的 tip 数量表示不断增长的未经批准的事务数量。

(3)假设事务在被添加到 DAG 时被执行。节点之间的事务执行顺序将显著不同，因此使得 DAG 对于某些应用无用，这些应用需要关于执行顺序的某些保证，例如需要在轮询结束之前到达的轮询中的投票。

(4)这也将防止 DAG 的删减，因为没有稳定的状态，节点可以快照(在没有单个协调器节点的情况下)以便删减所有先前的事务历史。

> [在您的收件箱中直接获得最佳软件交易](https://coincodecap.com/?utm_source=coinmonks)

[![](img/7c0b3dfdcbfea594cc0ae7d4f9bf6fcb.png)](https://coincodecap.com/?utm_source=coinmonks)

总的来说，我怀疑 DAG 在事务到达时确认事务的能力导致它对事务传播延迟和事务到达节点的顺序变得更加敏感，这在负载下可能导致未确认事务的累积和 DAG 成长为类似云的形状，其中参考旧 tip 的事务的接受不是例外，而是规范，并且不断增加的 tip 数量仍然未被批准。

[![](img/e7b1dbc6a532a697c6844fdf0f0bbd30.png)](https://coincodecap.com)