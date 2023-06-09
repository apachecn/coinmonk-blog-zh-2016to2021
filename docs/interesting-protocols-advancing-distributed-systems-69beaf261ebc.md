# 推进分布式系统的有趣协议

> 原文：<https://medium.com/coinmonks/interesting-protocols-advancing-distributed-systems-69beaf261ebc?source=collection_archive---------5----------------------->

![](img/e7a7ba5f5edbd7c226dfd06e8666f7ac.png)

Photo by [Marius Masalar](https://unsplash.com/@mariusmasalar?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

> “技术是一个描述还不能工作的东西的词。”—道格拉斯·亚当斯

分布式分类帐的世界是巨大的，有些是可行的，有些只是存在于理论中，没有预先的媒介来允许创造力、数学、头脑在现实世界的应用中被执行。在我个人看来，我们已经达到了一个临界点，不仅仅是块顺序连接，这就是 2008 年。我们有各种类型的分布式账本在我们的指尖，所有承诺的世界。有向无环图(Dag)，Hashgraph——到处都是流言蜚语，甚至多维区块链承诺并行化和跨链即时通信。

如果我们都后退几步，抽象出预期的价值，我们会在链条的末端意识到，可以说，它们都代表了一个国家转移的系统——资产、价值，任何头脑能够想到的东西。有些更快，有些更安全，有些是为私人企业设计的。

爱德华·泰勒曾经说过:“今天的科学就是明天的技术”。链、块、顺序链接——看，我们明白了，它不再令人兴奋。你想问什么？这里有一些概念，不完全是刚出版的，但是我发现很有趣，希望你也一样。它们是今天的科学，理论现在被赋予了探索的媒介。这些都是协议、算法治理、规模方面的进步，它们将单独或共同引发进一步的辩论——再次让技术专家兴奋。不管是支持还是反对，它们都可能是明天的技术。

## **子弹证明**

![](img/bd0a3bb3f6ce7d663c6054a9618a6f40.png)

Photo by [Spenser H](https://unsplash.com/@spensewithans?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

比特币以其假名性质而闻名，它具有匿名性——现实世界中的人与比特币地址没有联系。但是缺少隐私交易的另一部分—保密性，因为它暴露了交易中的金额。从脸书泄密到爱德华·斯诺登，隐私和数据一直是热门话题。人们想要它，不是因为他们是罪犯，因为他们相信他们有远离监控，自由生活的权利。一个在这个数字时代几乎不可能生存的完美世界。

Bulletproofs 就是这样一种隐私技术，由来自斯坦福大学、伦敦大学学院和 Blockstream 的六位高智商人士提出。它们提供了一种计算量小、保密的交易，能够被现有的连锁店相当容易地(有争议地)采用。它不像 BTC 那样提供匿名，因为它的用例是不同的。然而具有许多有价值的好处。

与知识系统的另一个零知识论证 ZK-斯纳克不同，子弹证明是轻量级的，不需要可信系统。这些被称为**“简短的非交互式零知识证明”**，它们在传输或存储它们的任何分布式系统中都有宝贵的好处，因为它们降低了总成本(防弹 WP，pg2)，如交易费，根据 Monero 的建议，它可以节省多达 80%。白皮书指出，在撰写本文时，比特币区块链有 160 GB 的范围证明数据，如果使用 Bulletproofs，则只有 17 GB，减少了近 10 倍。令人印象深刻。

这项技术还处于萌芽阶段，还没有在大规模系统中得到验证，尽管它已经得到了密码社区的大力支持。如果你热衷于检验它，并且是一个数学天才，点击[这里](https://eprint.iacr.org/2017/1066.pdf)。

## **MIMBLEWIMBLE**

![](img/d9bbf18aa67a1ed74c33999a0a85d662.png)

Photo by [Mervyn Chan](https://unsplash.com/@mervynckw?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

那一年是 2016 年，一个笔名作者“汤姆·埃尔维斯·杰杜索尔”(伏地魔在*哈利·波特*小说法文版中的真名)。登录一个 BitcoinTalk 论坛，丢下一份白皮书，然后就消失了。也许是 Satoshi，给加密货币世界留下了另一份礼物，在里面，一个激进的新提议精简了比特币协议。它直面隐私和可伸缩性。

该提案建立在现有协议的基础上，如“硬币连接”,允许用户将他们的交易捆绑成一个大的交易，扰乱所有的输入和输出——当然是为了隐私。通过使用所谓的彼得森承诺方案，创建者“汤姆”将这种方案的应用与比特币目前的使用方式相反，并使用它来生成伪签名，同时仍然能够证明所有交易都是有效的。隐私和可替代性的巨大飞跃。

在这样做的时候，它还实现了通过只记录 UTXO 和创建块来有效地最小化区块链膨胀和大小的能力——就是这样。在整个生命周期中，存储在链上的事务不是很长。因此，当前输出的记录是唯一重要的因素，如果有人能证明这是正确的话——快乐的日子。

在上面的 Bulletproofs 中，直接引用了白皮书中关于 mimblewimble 的内容，并在这两种技术之间建立了一个有趣的交集；

> “一个 mimble 区块链随着 UTXO 集的大小而增长。使用 Bulletproofs，它只会随着具有未用输出的事务数量的增加而增加，这比 UTXO 集的大小要小得多。总体而言，Bulletproofs 不仅可以作为保密交易中范围证明的替代方案，还可以帮助 Mimblewimble 成为一个实用的方案，其区块链远远小于当前的比特币区块链。”

当然还有更多，这只是向你介绍我觉得有趣的概念。因此，请点击查看[。](https://download.wpsoftware.net/bitcoin/wizardry/mimblewimble.txt)

## 保险箱

![](img/c7353b668adcc345d24831c00f4aa6a8.png)

Photo by [Samuel Zeller](https://unsplash.com/@samuelzeller?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

围绕可扩展性、解决方案以及我们应该如何做的争论已经持续了多年。比特币现金走了一条简单的路线，增加了块的大小，几乎没有创新，并慢慢地导致模仿货币在他们的路线图上进一步集中化到 1GB 块。当摩尔定律适用于晶体管而非存储器时，这是一个糟糕的解决方案。还是一个没有生命的概念。

保险箱是阿尔伯特·莫利纳的发明，他是帕斯卡硬币的创造者。不需要个人下载完整的区块链，如果你现在想要比特币，需要 160GB+。保险箱只需要一个人下载最后 100 块。这是通过存储余额来实现的，而不是像比特币那样存储事件的分类账，这也是 SAFEBOX 和 Mimblewimble 的交集。

正是这种架构，这意味着理论上块大小是动态的，如果与比特币节点运营商当前存储的数据进行比较，SAFEBOX 可能会有 5.4GB 的块，吞吐量为 72，000 万亿次(根据他们的白皮书)。这都是相对于比较而言的，而不是当前发生的事情，但它显示了保险箱可伸缩性的强大用例。因为目前它在生产中实现了超过 100 TPS。

总的来说，这是一个可删除的区块链，保持了轻量级和可伸缩性。来看看[这里](https://pascalcoin.org/)

## 量子力学——随机数生成

![](img/c5c560b1e4e50d3b7d75b27a4e2ffa94.png)

“Lines of code on a computer screen” by [Lewis Ngugi](https://unsplash.com/@ngeshlew?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

随机性是人类特别讨厌的东西，比如选择吃什么食物，甚至创造新的密码。当谈到区块链和加密货币的世界时，一些人甚至可能选择生成自己的大脑钱包。基于以上所述，不完全是最好的主意，因为恶意行为者可能会使用字典攻击来窃取您隐藏的比特币。

尽管在整个分布式系统中，使用了诸如工作证明(POW)、利害关系证明(POS)和拜占庭容错(PBFT/IBFT)等一致算法，但对随机性的需求是与生俱来的。在随机数生成方面。

在这种分布式系统中，它用于确保不推断某些事情，人们不能简单地猜测下一步是什么，无论是选择节点以在 POS 中运行共识协议还是创建助记符(种子短语)。

这方面的一个关键挑战是，很难确保发电的产出实际上是不可预测的。最近使用无漏洞的贝尔测试取得了进展，该测试不可否认地基于任何已知的禁止比光更快的物理理论(超光速信号)。尽管还有很长的路要走，生成起来很耗时，也很难实现，但在这种情况下进行的测试肯定会为如何使用量子力学来降低量子计算机在保护未来分布式系统方面的原始能力铺平道路。

这篇最近的研究文章可以在这里找到[(只提供摘要)。](https://www.nature.com/articles/s41586-018-0019-0)

## **共识算法**

这些在很多方面都不是新的或新奇的。拜占庭将军问题是莱斯利·兰波特在 1982 年提出的。在此后的几年里，我们看到了许多迭代，包括:

1.  [实用拜占庭容错](http://pmg.csail.mit.edu/papers/osdi99.pdf) (PBFT) — 1999
2.  [伊斯坦布尔拜占庭容错](https://github.com/ethereum/EIPs/issues/650) (IBFT) — 2017
3.  [嫩薄荷](http://tendermint.readthedocs.io/projects/tools/en/master/index.html)
4.  [工作证明](https://bitcoin.org/bitcoin.pdf)(粉末)
5.  [股权凭证](https://github.com/ethereum/wiki/wiki/Proof-of-Stake-FAQ) (PoS)
6.  [股权委托证明](https://themerkle.com/what-is-delegated-proof-of-stake/) (DPOS)
7.  涟漪

然后奇怪和美妙的名单滚动；

8.储存证明(PoS)

9.授权证明(PoA)

我跟不上我真的跟不上，从拯救网络免受 Sybil 攻击，到整体 51%的腐败。这些共识算法是为了各种各样的目的而产生的……但我可以有把握地说，PoW 是分布式分类账中唯一能经受住时间考验的长期算法。

# 剧终

一个让我思考分布式系统有趣工作的小清单。希望你能从中获得一些研究成果！

本杰明·霍尔。

联系！

YouTube:[https://www.youtube.com/c/cryptocatchup](https://www.youtube.com/c/cryptocatchup)

推特:[https://twitter.com/crypto_catchup](https://twitter.com/crypto_catchup)

领英:[https://www.linkedin.com/in/benjamin-hall-760b9891/](https://www.linkedin.com/in/benjamin-hall-760b9891/)

网址:[https://www.cryptocatchup.com/](https://www.cryptocatchup.com/)