# 剖析 51 %的双重花费攻击

> 原文：<https://medium.com/coinmonks/anatomy-of-a-51-double-spend-attack-b5ac9080fe80?source=collection_archive---------4----------------------->

也许你已经听说了最近对比特币黄金交易所的攻击，攻击者使用双重消费攻击的形式窃取了一些 BTG。

![](img/ac41eadca8071f65c73ebf670866666c.png)

现在，你可能开始怀疑:这怎么可能？区块链的天才之处就在于防止这种类型的攻击。但是这一重要特性仅在特定的网络条件下成立。让我们探讨一下这种攻击是如何可能发生的，以及您可以做些什么来防范它。

# 什么是双重消费？

双重花费问题是任何电子代币交换系统的问题，其源于资产的数字性质。当一个东西是数字的，这意味着它可以被复制，你无法区分副本和原件。双重消费简单地说就是，你复制一份数字资产，并试图同时消费原件和副本(可能在不同的商店)。为了防止这种类型的攻击，您必须确保令牌只能使用一次。

有两种方法，如何应对这一点:集中和分散。在中央系统中，每个人都必须向一个可信的第三方注册，这个第三方保存资产的中央注册表，并确保不会发生重复花费。这种系统的一个例子是中央股票存管机构，它保存着每个拥有公司股票的人的登记。如果我想向 Bob 出售一股苹果股票，注册中心首先验证我至少有一股，然后在一个原子事务中减少我的股票数，增加 Bob 的股票数。我不能把同样的股份卖给任何人。

但是去中心化的系统呢，比如比特币？这就是区块链的用武之地。这里有三个基本要素。

1.  每个人都能看到每笔交易，每个人都可以验证交易在密码上是有效的。
2.  事务的顺序由不断增长的块链接链决定，这意味着块 1 中的事务肯定发生在块 2 中的事务之前。一旦这个链中有另一个块，这个链就不能被篡改。本质上，如果不交换跟在它后面的所有块，就不能替换任何块中的任何事务。
3.  交换区块是非常昂贵的，因为有效区块只能通过投入大量工作(能量)来开采才能产生。

这个概念在 Nakamoto 最初的论文中发表时是革命性的，并且是第一个有效解决不可信的分散系统中的重复花费问题的方法。

# 为什么是“51%”？

51%的比率指的是在给定的工作证明方案中，一个矿工控制超过 51%的总散列能力的情况。这意味着，一个实体能够在工作证明系统中生成比其他所有实体加起来还要多的工作。这本身并不危险。毕竟，矿工不能产生涉及矿工不控制的地址的有效交易。他们无法从你的私钥中窃取你的比特币，也无法通过这个系统窃取任何其他硬币。矿工只能决定是否在块中包含单个交易。然而，这种情况可以(而且曾经)被利用，我们将在后面进一步描述。

# 发动攻击

那么，如果恶意矿工不能通过产生交易来偷钱，他们是怎么做到的呢？我没有关于他们实际上是如何做到的信息，但是我将描述这样的攻击是如何进行的。

主要的先决条件是，恶意挖掘者必须始终控制 give 链上 50%以上的散列能力。比率越高，成功的几率就越大。理论上 51%应该够了，如果能维持更长时间的话。

在某个时候，比如说 500000 号区块，矿工开始使用他的多数哈希权力私下开采区块。他不将这些块发布到主网络。积木可以是空的，这并不重要。

与此同时，在 500001 块中，矿工使用主网络，在一个流动交换上存入一笔可观的存款，比如说从他的地址 A 存入 100 BTG。交易所通常会等待几个确认(换句话说，在包含交易的块之后挖掘的块)。让我们假设他们等待 10 次确认，这或多或少是替代硬币的标准。我们现在位于主网络的街区高度 500011。

存款得到确认，矿工现在将 BTG 兑换成 BTC(或任何其他货币)，并在比特币网络上立即取款。假设我们在街区高度 500012。现在是触发陷阱的时候了。

因为矿工控制着多数股权，他秘密开采的私人链条比主网上的 500012 还要长。正如我所说的，块可以是空的，但有一个事务是矿工肯定包括的，这是一个事务，它将 100 BTG 从地址 A 转移到矿工控制的另一个地址。这是双重花费交易，因为在将资金发送到交易所时，来自地址 A 的硬币已经被花费了。

矿工现在将他私人挖掘的链中的所有块提交给网络，并且因为它比主网络上的链更长，例如它可能在块高度 500014 处，所以它被其他节点自动接受为一个真正的链。块 500001 到 500012 中的所有事务都被无效并返回到内存池。这也包括将钱从地址 A 转移到交易所的交易。但是因为在新的有效链中已经有一个从 A 到另一个地址的交易，所以这个交易被标记为双重花费并被丢弃。

交易所没有 100 BTG，因为该交易被回滚，并且也被抢走了交换的比特币，因为它在攻击公开之前被撤回。嘣！

# 预防

悲伤的部分来了。事实上，交易所在防止这种抢劫方面无能为力。这只能使它变得更加困难，但是会给用户带来不便，并且它可以采取措施来检测这种攻击。

在认为保证金有效之前，交易所可以增加要求确认的次数。当然，这也增加了合法用户的等待时间。

另一种常用的保护措施是延迟取款。如果交易所在比特币兑换后扣留了它，它可能会否认取款，而矿工将一无所获。但是，这又给合法用户带来了不便。

交换机还可以监视网络散列值的突然下降。这是矿工用一半以上的 hashpower 开始开采自己的私链造成的。在矿工之前挖掘其他硬币并且在转换硬币之后立即开始挖掘私有链的情况下，这种检测是无效的。

# 比特币安全吗？

如果比特币黄金不堪一击，那么真正的比特币安全吗？还好答案是*是*。比特币黄金的问题与许多其他小型替代硬币类似，即它与一个更大的网络共享算法。BTG 使用的是 Equihash 算法，与更受欢迎的 Zcash 算法相同。因此，如果一个矿工控制了 10%的 Zcash 哈希权力，Zcash 网络的总哈希权力是 1000，比特币黄金的总哈希权力是 50，突然将这 10%的权力从 Zcash 导向 BTG，使矿工控制了 66%的 BTG。在这种情况下，Zcash 不会受到这种攻击，但是较小的网络却不能。

同样的事情可能发生在任何使用与比特币相同的工作证明算法的替代币上。如果你拥有一家像样的比特币挖矿公司，控制 1%的比特币 hashrate，你完全可以接管这样的 altcoin。当然，在你使用你的采矿设备在 altcoin 网络上扮演上帝期间，你会失去比特币中的采矿奖励。

这很可能是这里发生的事情。如果一个大玩家开始在比特币或 Zcash 等大型网络上积累散列能力，人们会注意到。几年前，比特币也发生了类似的情况，这种情况很快得到了解决，因为即使是矿工也意识到，如果有人持有超过 51%的股份，这将破坏整个生态系统的稳定，他们在采矿设备上的重大投资将会贬值。

这也是为什么比特币是最安全的加密货币。原因很简单，因为无论如何都很难影响。如果您正在处理替代硬币，请始终检查是否在一些更大的网络上没有使用相同的 PoW 算法，并相应地调整您的确认要求。