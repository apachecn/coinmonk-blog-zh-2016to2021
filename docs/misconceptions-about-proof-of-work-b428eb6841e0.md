# 关于工作证明的误解

> 原文：<https://medium.com/coinmonks/misconceptions-about-proof-of-work-b428eb6841e0?source=collection_archive---------9----------------------->

## 矿工在建造区块链时没有选择的自由

*作者注:这应该作为我文章的附录来读:* [*我们不需要杀手机器人，我们已经有比特币了。*](/@Wabi-Tree/we-need-no-killer-robots-we-already-have-bitcoin-7f029d27300d)

当我试图理解比特币和区块链技术的工作原理时，有几个关于工作证明的误解阻碍了我。

我最初曾想过在我的文章中加入这一摘录，但后来决定不这么做，因为它分散了讨论的注意力，并使事情变得混乱。相反，我把它作为一篇独立的文章发表，因为它是一个独立的主题。

![](img/99646d2e2d755a0b5a0ead6a09333f7d.png)

Photo by [Element5 Digital](https://unsplash.com/@element5digital?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/voting?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 矿工 vs 节点

首先，需要明确区分矿工和节点。矿工也是节点，但他们是专门的节点，因为他们执行额外的采矿任务，努力向区块链添加区块。

严格来说，矿工的角色是**而不是验证交易的合法性**。相反，验证是完整节点在被选入块之前进行的预检查的一部分。

即便如此，这些节点的预编程检查主要是通过将余额与最新的分类账进行比较来防止账户超支。从广义上讲，你的数字签名(或你的私人钱包地址)实际上是抵御欺诈性钱包使用的防线。

换句话说，如果你的私人钱包地址被泄露和窃取，黑客可以在未经你允许的情况下将 BTC 转移出你的账户。从网络节点的角度来看，这些“欺诈性”交易与分类账记录相匹配，有资格被解析为一个块。

一旦进入一个区块，就有一定的机会将这些交易计入区块链分类账。

# 区块链中的民主

另一个常见的误解是，矿工可以通过“投票”选择正确版本的区块链来行使他们在区块链的代理权。这种情况不会发生。

这种不正确的解释通常出现在临时分支的主题中，当两个或更多的矿工同时解决他们的区块时，会导致关于哪个区块应该是一致区块的争论。在这里，最长链原则开始起作用，它指出，具有最多工作证明的块将是最终的赢家，而其他块(及其后续链)将成为孤儿。这实质上意味着，如果一条链上的矿商可以建造得更快，他们就会使最初有争议的区块合法化，并迫使他们的链达成共识。

在这个次级竞赛中，矿工不能选择队伍。由于广播滞后，矿工可以收到不同的获胜区块的结果通知。临时岔道的出现对矿工来说无关紧要。由于速度是游戏的名称，矿工只需要集中精力在广播给他们的第一个区块上采矿，就有最好的机会站在“正确”的链上。

从本质上讲，矿商无法判断区块链区块的价值，也无法有意识地决定“支持”哪条链。相反，他们的行为受到协议框架和货币化激励制度的隐性控制和支配。

> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)了解加密交易和投资