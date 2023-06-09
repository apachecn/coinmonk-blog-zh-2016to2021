# 如何在聚会上变得有趣:了解区块链教授

> 原文：<https://medium.com/coinmonks/how-to-be-fun-at-parties-understanding-blockchain-professor-86a3c1a2bb0f?source=collection_archive---------8----------------------->

# 概观

如果你来自《如何在派对上变得有趣:了解区块链——学徒》一书，或者如果你想了解区块链和以太坊内部技术运作的一些业余爱好，这篇文章是为你准备的，是三篇文章中的第三篇。如果你对你的知识感到不舒服，请随意跳回到这个系列的开头[如何在聚会上变得有趣:理解区块链——学生](/@james_64667/how-to-be-fun-at-parties-understanding-blockchain-student-a7653fda39b9)并继续阅读。本文将带您了解以太坊当前面临的一些挑战，以及考虑到这些问题，以太坊的未来会是什么样子。

# 链环

## Oracle 问题

oracle 是一种信息来源，允许在区块链网络内部和外部之间传输信息，因此对于某些智能合同的功能至关重要。神谕可以承担不同的任务，因此有不同的类型，如；软件 oracle、硬件 oracle、入站 oracle、出站 oracle 或一致 oracle。oracle 也可以是某些服务类型的组合(例如入站和软件 Oracle)。

如果为通知智能合同的操作而收集的信息不正确或不足以允许任何有意义的操作发生，则可能会发生意想不到的后果。如果没有适当程度的信息可供参考，网络还可能难以检测欺诈性数据块，或者在遇到挑战时难以确定两个数据块中哪一个是有效的。

神谕的固有问题是它们需要信任，这违背了去中心化的区块链网络的关键原则。如果一个 oracle 受到损害，那么任何依赖于该 oracle 的输入来执行其契约代码的智能契约也会受到损害。甲骨文的问题已经讨论了很长时间，许多不同的贡献者，包括 Chainlink，提供了不同的解决方案。对于 oracle 问题，似乎没有单一的解决方案，即使使用一致的 Oracle 也无法保护契约免受攻击者的蓄意攻击，攻击者可以提交如此多的假值，以至于正确的值看起来像是离群值。

Chainlink 允许区块链获得真实世界的数据，以执行智能合同。最好把 Chainlink 想象成一个分散的神谕网络。Chainlink 网络不仅向链上智能合同提供数据，而且还聚合结果以有利于提供给合同的数据的准确性，并保持 oracles 的信誉及其数据有效性的记录。Chainlink 可以与各种区块链一起使用，并且可以接受各种信息源(多种类型的 oracles)。

## 体系结构

Chainlink 流程有三个组件:请求智能合约、Chainlink 智能合约和链接到外部 API 的 Chainlink 节点。当请求智能合约在区块链上向链式智能合约请求执行智能合约所需的信息时，启动该过程。然后，Chainlink 智能合约通过记录 oracles 网络的事件，将区块链连接到 Chainlink 节点。Chainlink 智能合约能够收集 Chainlink 节点在完成作业时前进和收集的所有数据，并将其整理成原始请求智能合约易于阅读的形式。

软件神谕通常包括公共网站和数据库等在线资源，其特征是与互联网的连接。与此同时，硬件神谕与现实世界相连，可以从超市收银台扫描条形码等事件中获取信息。入站 oracle 是智能合约的信息源，它是一种纯粹的单向关系，也是智能合约从其区块链内部或外部接收信息的唯一方法。出站 oracle 服务于相反的目的。它从智能合约接收信息，本质上充当它的代言人。还是那句话，这纯粹是单向关系。

一个一致的先知可以访问一系列的信息来源，并将这些信息来源的一致意见视为真实准确的信息。这方面的一个例子是从多个气象站获取记录温度的神谕。这样，如果一个站的测量值不正确，先知的完整性不会受到损害，因为如果其他站同意彼此交换，那么先知会考虑大多数人认为是正确的。

## Oracle 选择

当选择 oracles 用于智能合约的请求作业时，会提出服务级别协议(SLA)建议，其中包括请求智能合约希望满足的一些要求。这些需求主要由所需的先知数量和先知的期望声誉组成。创建智能合同的用户能够通过各种链外列表服务手动排序和选择 oracles。一旦用户完成 SLA，他们将把建议提交给订单匹配合同，该合同将依次联系 oracle 服务提供商监控的作业日志。一家 oracle 服务提供商将决定是否参与投标。如果它确实想要这个提议，它将通过承诺一个罚值来出价。有一段时间被称为投标窗口，在此期间所有投标都被接受和考虑。在投标窗口结束时，订单匹配合同将拒绝不符合 SLA 要求的投标，并选择最符合所述要求的所需数量的 oracle 服务提供商。所有不成功的投标都将被返还其罚金值。成功的 oracles 服务提供商得到通知，并开始完成 SLA 中概述的任务。

## 整理结果

oracle 将把它们的数据报告给 oracle contract，Oracle contract 再把数据报告给一个聚合合同。汇总契约的作用是记录不同的响应并计算出一致意见。共识通常是通过对有信誉的先知进行加权来形成的，然后这个共识被发送到最初请求该工作的智能合同。每个先知都要对他们报告的数据负责，他们的数据与共识相比的准确性将报告给信誉合同。对于 oracles 来说，没有单一的聚合契约可供报告，因为不同的数据类型对于异常值的构成会有不同的标准，因此所使用的聚合契约由用户选择。Chainlink 提供了一组标准的聚合契约供选择，同时还允许实现自定义契约。

# 以太坊 2.0

伊斯坦布尔和柏林的硬分叉表明了以太坊 2.0 的许多意图，但这些分叉并不是唯一旨在影响不久的将来即将到来的以太坊 2.0 更新的分叉。以太坊的这些变化已经并将会改变区块链网络——其中最重要的变化可能是转向使用利益相关证明共识机制，而不是工作证明共识机制。

## 伦敦硬叉

第二个最近的硬分叉被推到以太坊上；这些累积的变化旨在帮助以太坊从以太坊 1.0 发展到以太坊 2.0——最有可能在 2022 年的某个时候。最近的硬分叉本身，Muir Glacier，被用于将以太坊 2.0 的合并事件推迟到明年晚些时候的主要目的。

伦敦硬分叉中的一些更改包括 EIP-1559(EIP 或以太坊改进提案，是由社区提出的更改，由社区投票决定它是否会像政府中的立法一样实施)。这种 EIP 为用户需要支付以将他们的交易包括在区块中的燃气费提供了更大的稳定性，并且在区块的大小方面提供了更大的灵活性。另一个值得注意的变化是纳入了 EIP-3554，它将合并的预计日期向后推迟了(这是在以太坊上工作证明逐步淘汰股权证明的情况)，甚至在穆尔冰川硬分叉将它推得更远之前。

## 分片

分片将允许 L1 以太坊的多个“碎片”或链连续运行，每个碎片运行主区块链的一个小版本。这意味着验证器不需要主区块链的完整副本(以及保存该副本所需的硬件要求)就可以验证特定碎片上的块。这将允许更多的验证者加入网络，而不管他们的计算能力如何，从而大大增加网络的安全性。

将有一个主要的“信标链”,它将跟踪所有碎片，并确保网络上所有实体的所有碎片都保持完整性和可用性。

## 利害关系证明

以太坊 2.0 打算在未来几年完全从任何工作证明转移到利益证明。在过去的几年里，以太坊一直在采取激励措施，鼓励实体将他们的股份转移到一个已经在股份证明上运行的分支上，这样当完全转移发生时，以太坊就不会突然崩溃。

这一举动的动机包括希望减少采矿区块所涉及的能源使用(以及采矿所涉及的增加的天然气成本),并保护以太坊免受集中化的影响，这一点随着某些实体控制越来越多的计算能力而变得越来越明显。这些变化是有争议的，因为许多实体将突然失去他们为获得高度计算能力而投入的所有投资。

与工作验证不同，利害关系验证还没有经过足够程度的“战斗测试”,以确定它在切换时会对以太坊产生什么影响。虽然人们普遍认为它将在推出后立即减少集中化，但有人担心集中化最终仍会发生，因为只有有限数量的实体控制着成为股权证明验证者所需的股权。目前，最低持股比例为 32 ETH(截至 2021 年 12 月，大约相当于 180，000 澳元)，尽管随着以太坊 2.0 达到更高的稳定性，这一比例将会逐渐降低。

# L2 解决方案

目前，L2 以太坊的未来主要有两个可行的竞争者。下面将更详细地介绍两种 L2 解决方案的规格及其局限性。

这两种 L2 解决方案都采用了在主 L1 区块链外处理块的形式，以减少在那里进行的计算工作。在它们被离线处理后，它们被添加回主链以进行验证和可能的质询。这种模式提供了减少处理块所涉及的 gas 成本的优势，因为总体上完成了较少的计算工作。

## 乐观汇总

在乐观汇总的情况下，假设已添加的块是真实的，直到证明不是这样。这是有利的，因为在最坏的情况下，验证通过乐观汇总添加的块所需的总计算工作是最小的。如果该区块最终被某个实体质疑，则主区块链将不得不运行大量的计算工作来验证该区块是诚实的，这将增加天然气成本。然而，这些气体成本被传递到挑战的“失败者”身上，阻止了欺诈性区块被添加，并阻止了欺诈性挑战。

这种 L2 解决方案实施起来简单得多，并且在降低以太坊网络的天然气成本方面显示出立竿见影的效果。因此，大多数公司都把这个解决方案作为他们以太坊中短期战略的一部分。

## ZK 汇总

就降低油价而言，ZK 卷有望成为最有效的 L2 解决方案，同时又不会降低区块链的安全性。允许他们工作的是一组难以计算的证明，但验证起来很简单，不需要共享块本身涉及的任何数据。在乐观汇总中，几乎不需要进行验证，但必须接收所有数据，这确实会略微增加天然气成本。然而，在 ZK 汇总中，需要进行少量的验证，但是不需要接收数据，这确实进一步降低了天然气成本。

虽然允许这一切工作的数学还没有完善，但据信至少需要一年或更长时间才能将 ZK 卷应用到以太坊的最佳质量。因此，大多数公司不会在短期内寻求这种解决方案，但他们意识到，他们需要为其长期的潜在到来做好准备——可以说是两个 L2 解决方案中最有优势的一个。

# 结束语

通读这整个系列，包括[如何在派对上变得有趣:了解区块链——学生](/@james_64667/how-to-be-fun-at-parties-understanding-blockchain-student-a7653fda39b9)和[如何在派对上变得有趣:了解区块链——学徒](/@james_64667/how-to-be-fun-at-parties-understanding-blockchain-apprentice-d4cbee2a6af2)(或者至少是你需要完善你的区块链和以太坊知识的部分)，希望你对区块链可能影响的未来有更好的准备。每天都有越来越多的政府和企业在决策中采纳和采纳区块链教。让公众了解所有这些变化对他们意味着什么是很重要的，因为这是区块链社区的一个重要支柱，即一切都是分散的，所有用户都有发言权。谢谢你，我祝你一切顺利，无论未来是什么样的。

> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)了解加密交易和投资

## 也阅读

[](/coinmonks/leveraged-token-3f5257808b22) [## 杠杆代币[多头代币]终极指南

### 杠杆化令牌是具有杠杆化风险敞口的 ERC20 令牌，不考虑保证金、要求、管理…

medium.com](/coinmonks/leveraged-token-3f5257808b22) [](https://blog.coincodecap.com/crypto-exchange) [## 最佳加密交易所| 2021 年十大加密货币交易所

### 加密货币交易所的加密交易需要了解市场，这可以帮助你获得利润。之前…

blog.coincodecap.com](https://blog.coincodecap.com/crypto-exchange) [](https://blog.coincodecap.com/best-swap-platforms) [## 2021 年最佳加密交换平台| CoinCodeCap

### 如果我们看看今天的场景，许多加密货币交换平台提供了广泛的功能和深度…

blog.coincodecap.com](https://blog.coincodecap.com/best-swap-platforms) [](/coinmonks/top-5-crypto-lending-platforms-in-2020-that-you-need-to-know-a1b675cec3fa) [## 2021 年最佳加密借贷平台| 6 大比特币借贷平台

### 获得比特币和其他加密货币的最佳贷款利率

medium.com](/coinmonks/top-5-crypto-lending-platforms-in-2020-that-you-need-to-know-a1b675cec3fa) [](/coinmonks/the-best-cryptocurrency-hardware-wallets-of-2020-e28b1c124069) [## 2021 年 6 大最佳硬件钱包|顶级加密硬件钱包[更新]

### 最好的加密货币硬件钱包是绝对必要的。我们将在 NGRAVE、Ledger Nano X 和…

medium.com](/coinmonks/the-best-cryptocurrency-hardware-wallets-of-2020-e28b1c124069) [](/coinmonks/crypto-trading-bot-c2ffce8acb2a) [## 2021 年最佳免费加密交易机器人

### 2021 年币安、比特币基地、库币和其他密码交易所的最佳密码交易机器人。四进制，位间隙…

medium.com](/coinmonks/crypto-trading-bot-c2ffce8acb2a) [](/coinmonks/best-crypto-signals-telegram-5785cdbc4b2b) [## 最佳 4 个加密交易信号电报通道

### 这是乏味的找到正确的加密交易信号提供商。因此，在本文中，我们将讨论最好的…

medium.com](/coinmonks/best-crypto-signals-telegram-5785cdbc4b2b) [](https://blog.coincodecap.com/bitsgap-review) [## 获取信号、交易机器人和套利

### 编辑描述

blog.coincodecap.com](https://blog.coincodecap.com/bitsgap-review) [](https://blog.coincodecap.com/best-telegram-channels) [## 40 个最佳电报频道，用于加密、电影、表演和演讲| CoinCodeCap

### 编辑描述

blog.coincodecap.com](https://blog.coincodecap.com/best-telegram-channels)