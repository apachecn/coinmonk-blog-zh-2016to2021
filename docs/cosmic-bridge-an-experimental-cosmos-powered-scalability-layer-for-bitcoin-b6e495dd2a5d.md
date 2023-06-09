# 宇宙桥:一个实验性的由宇宙驱动的比特币可扩展性层

> 原文：<https://medium.com/coinmonks/cosmic-bridge-an-experimental-cosmos-powered-scalability-layer-for-bitcoin-b6e495dd2a5d?source=collection_archive---------6----------------------->

# 探索宇宙

今年早些时候，我们( [Chris Buonocore](https://medium.com/u/691d2efc68c4?source=post_page-----b6e495dd2a5d--------------------------------) 、 [Darwin Li](https://medium.com/u/2f0e2c1a42?source=post_page-----b6e495dd2a5d--------------------------------) 和我)在三藩市的 C4YT(思想密码)黑客马拉松上发现了 [Cosmos](https://cosmos.network/) 。宇宙是一个值得大肆宣传的项目；我们对其深思熟虑的设计和区块链连通性的愿景印象深刻。

然后，我们发现了锦上添花:[乳液](https://github.com/keppel/lotion) ( *注:请不要在你的蛋糕上放实际的乳液*)。Lour 利用[tender mint](https://tendermint.com/)(Cosmos 核心的共识引擎)允许用简单的 Javascript 代码轻松创建独立的区块链应用程序。

Lour 是一个非常棒的区块链应用原型开发工具——很快，随着它的成熟和 Cosmos 自身的成熟——它将会开发出高质量的区块链应用。目前，这是在一般的区块链应用程序上试验&迭代的最快方法之一。

在黑客马拉松中，我们为在线媒体建立了付费墙和微支付解决方案。作为一名用户，你首先要在一张预付的“媒体卡”中装入一些比特币，然后你就可以用这张卡在参与发行商的网站上进行比特币小额支付。这实现了无摩擦的内容付费墙，这是支持高质量新闻的一种机制。

由于比特币的交易成本使得链上小额支付成为不可能，我们的解决方案使用乳液来管理小额支付的侧链，这可以实现便士级别的支付。我们提供了一个端到端的解决方案，包括浏览器内用户体验、所需的比特币集成(由了不起的 [bcoin](http://bcoin.io/) 项目提供支持)和启用 Cosmos 的簿记，所有这些都在一个产品中。

用严格的专业术语来说，我们的解决方案非常好！尽管如此，它还是狭隘地专注于媒体网站的付费墙，所以 Cosmos 团队建议我们考虑更大、更令人兴奋的东西。我知道你现在在想什么:“还有什么比付费墙更令人兴奋的呢？”

# Cosmos 作为比特币的可扩展性层

当我们考虑可能性时，我们意识到没有理由将可扩展的交易解决方案局限于付费墙，甚至微支付。我们很快想到了一个问题:由 Tendermint 驱动的区块链能否被用作比特币的第二层，在提供可扩展性的同时仍然保持区块链的理想属性(不变性、可审计性和不可信任性)？

## 等等，什么…？比特币的第二层？

或许有一句插话可以解释为什么比特币的第二层是完全必要的。比特币的可扩展性和交易成本是一个相当深刻的问题，在其他地方已经详细讨论过[,但现在需要一个非正式的快速回顾。如果您熟悉这个问题，请不要介意跳过这一部分。](https://en.wikipedia.org/wiki/Bitcoin_scalability_problem)

比特币区块链的工作方式是，在给定的时间段内，链上只能进行一定数量的交易。随着比特币用户数量的增加，执行比特币交易的需求也相应增加，执行交易的需求超过了供应——实际上没有足够的“链条空间”来及时满足他们。

结果，越来越多的事务“排队”等待被包含。他们正在等待的线被称为*内存池*——等待在区块链上确认的交易的等待列表。每次挖掘一个块时，都是由块的挖掘者来选择从等待列表中挑选出哪些事务并包含在该块中。比特币协议允许交易的创建者通过支付费用来激励矿工首先加入他们的交易。

结果很容易猜测:人们为他们的交易支付越来越高的费用，因为否则他们会在等待列表(也称为内存池)中停留很长一段时间。这意味着比特币交易的实际成本会随着时间的推移而不断增加。

正如你可能猜到的那样，昂贵的交易对比特币的采用并没有那么好:如果通过比特币在全球范围内汇款变得昂贵，人们显然不会那么频繁地使用它。这也使得一些用例变得不可能，特别是小额支付:如果你需要支付 50 美分来访问一篇新闻文章，而仅交易就花费了 25 美分，那么整个用例是不可行的。

不幸的是，这个可伸缩性挑战的解决方案并没有达成一致。[可扩展性辩论](https://hackernoon.com/beginners-guide-to-bitcoin-s-scalability-debate-66060f3799e5)非常激烈，并导致了 2017 年夏天的比特币现金分叉。比特币核心(而非比特币现金)的维护者如今普遍认为，扩大比特币规模的解决方案是通过“第二层”——比特币区块链之上的额外一层。这样的层可以允许快速和便宜的交易，而不会在链上招致交易，以及相关的挖掘成本、等待列表机制等等。最被接受的第二层解决方案被称为[闪电网络](https://lightning.network/)。这是一个创新的点对点解决方案，已经在部署中，如果您还没有，我们鼓励您去看看。

![](img/83e78a80115cd44fa7298567e0131b23.png)

## 宇宙桥进入场景

为了扩大比特币的规模，需要第二层是相当确定的，而闪电是一项令人敬畏的技术。我们对 Cosmic Bridge 的想法是探索使用利益相关区块链作为第二层，因为区块链具有一些 P2P 解决方案所不具备的理想属性。更一般地说，它们只是代表了一组不同的设计选择。

我们就这样踏上了建造宇宙桥梁的旅程。宇宙桥背后的想法很简单:它允许任何人建立一个独立的区块链(宇宙术语中的“区域”)，可以用来进行比特币交易，几乎是免费的。我们称这样的区块链为“支付区”。

这种工作方式(大致)是通过让利害关系证明链的一组验证者成为比特币主链上多签名钱包的共同签名者。这个钱包保存着由支付区管理的比特币的总余额。用户可以将任意数量的比特币存入这个钱包——就像加载一张预付费借记卡一样——然后在支付区用加载的比特币进行交易，免费。余额会定期在比特币主链上结算，因此收款人最终会在主链上获得“硬”比特币。默认的结算周期是一个月，因此您获得了信用卡用户熟悉的每月结算的体验。

## 发展笔记

当我们开始开发这个项目时，Lotion 仍然处于开发的早期，我们不确定是应该在它的基础上构建还是直接使用 Cosmos SDK。然而，乳液的发展很快，我们意识到我们最好利用它，这样我们可以更快地前进。

Lour 允许你用最少的代码来试验和创建一个区块链应用程序，所以你可以很快地原型化和测试想法。尽管这个项目很新，但我们的经历很棒，每当我们有疑问或问题时，贾德·凯佩尔总是慷慨相助。我们鼓励您尝试一下——您可以在一个(非常)慵懒的下午创建自己的区块链应用程序。

作为一个整体，Cosmos 仍然是一个年轻且快速发展的堆栈，因此在设置和部署中肯定会偶尔出现一些小问题，但总体来说体验是令人愉快的，解决方案本身也很优雅。我们非常尊重 Cosmos 和 Tendermint 背后的团队。

比特币的整合，特别是多签名管理和适当的系统测试，也带来了自己的挑战——我们仍在努力解决一些问题，但 [bcoin](https://medium.com/u/2dc57baecd29?source=post_page-----b6e495dd2a5d--------------------------------) 已被证明是一个古老的工具。

总的来说，我们高度赞赏我们已经建立的项目。他们使我们能够相对容易地试验和构建 MVP。

# 用例

Cosmic Bridge 可以实现许多有趣的用例:

*   免费交易的比特币借记卡和商户网络
*   比特币小额支付解决方案
*   交易所或大型交易商之间的比特币流动性暗池

….还有很多。

虽然潜在的用例令人兴奋，并且可能非常有影响力，但事实是，当您构建基础架构项目时，更不用说实验性的概念验证了，您无法总是准确地预测您将在该领域看到什么用例。您可以在头脑中有一些可能的用例，以一种提供尽可能坚实的基础的方式设计解决方案，如果一切顺利，您最终会对人们提出的实际用例感到惊讶。

# 未来

看到令人惊讶的用例出现肯定会是一个很好的结果。我们认为，除了闪电(Lightning)等点对点解决方案之外，比特币在区块链的第二层可能对许多场景非常有用，因为它有一套不同的属性，可能更适合一些场景。有了 Cosmic Bridge，你得到了你所知道并喜爱的不可变账本，近乎零成本的交易，以及比特币主链上可预测的结算。作为回报，你用对一组众所周知的验证器的信任来代替完全的不信任(*注意，你信任集体，而不是任何一个单独的验证器——并且确信 Tendermint 的容错特性*)。

在某种程度上，上述关于基础设施项目的观察也适用于 Cosmos 本身。本质上是可扩展的，具有 Tendermint 和 [IBC](https://github.com/cosmos/cosmos-sdk/tree/master/docs/spec/ibc) (区块链间通信协议)的深思熟虑的设计支持各种用例，从现在熟悉的勺取和预期的分布式交换到许多其他我们尚未预见的情况。

至于宇宙桥，该项目仍处于实验 MVP 阶段，因此任何帮助都是受欢迎的！要了解更多关于设计和检查代码，请前往我们的 [github repo](https://github.com/CosmicBridge/server) 。*(提示:如果您正在寻找一个起点，为什么不帮助我们设计和实现验证器之间的安全* [*多签名协调协议*](https://github.com/CosmicBridge/server/wiki/Validator-Set-Multi-Sig-Bitcoin-Wallet%3A-Wallet-Creation-and-Transaction-Coordination-Design) *？😃)*

**感谢**[**ch jango Unchained**](https://medium.com/u/199d4ef6bb67?source=post_page-----b6e495dd2a5d--------------------------------)**和**[**Chris Buonocore**](https://medium.com/u/691d2efc68c4?source=post_page-----b6e495dd2a5d--------------------------------)**对本文的贡献！你真棒😎**

> [直接在您的收件箱中获得最佳软件交易](https://coincodecap.com/?utm_source=coinmonks)

[![](img/7c0b3dfdcbfea594cc0ae7d4f9bf6fcb.png)](https://coincodecap.com/?utm_source=coinmonks)