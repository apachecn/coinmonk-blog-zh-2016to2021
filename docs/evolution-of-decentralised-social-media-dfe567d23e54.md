# 去中心化社交媒体的演变

> 原文：<https://medium.com/coinmonks/evolution-of-decentralised-social-media-dfe567d23e54?source=collection_archive---------3----------------------->

除了用户直接支付区块链交易费

## 背景

分散式(公共)社交媒体的理念是，所有帖子都是公开的，不依赖于中央权威，并且没有删除。

帖子是使用区块链记录的，因此任何客户端都可以读写帖子。用户可以切换客户端，知道他们的帖子对任何客户端都可用。

区块链提供了一个分散的知识库，包括帖子的时间戳和作者证明。

这个概念可以用于微博、照片分享、长篇文章，甚至是版本控制库。

## P eepeth

[Peepeth](https://peepeth.com/) 是由[比万](https://peepeth.com/bevan)打造的微博平台。

在 Peepeth 之前，还有 [Leeroy](https://leeroy.io/i/featured/posts/top-tipped) ，是由[詹姆斯](https://twitter.com/childsmaidment)创造的，作为概念的证明。用户的每一个动作都是一次交易，因此需要支付区块链交易费。

Peepeth(第 2 版)是使用 Leeroy 开发的无状态智能合同概念构建的。包括帖子在内的动作都 [JSON 存储在 IPFS](https://gateway.ipfs.io/ipfs/QmdqHU7fYzbxqjUMu7EtutHRiHJsKxGCAWNG7EDfTNUaGn) 上。然后，IPFS 散列被存储为智能合约的交易输入。

用户可以为每个动作签署一个交易，因此为每个动作产生区块链交易费。

Peepeth 改进了这种体验，允许批处理 15 个动作(任意数量)。Peepeth 前端存储动作，然后用户可以批量提交这些动作( [JSON 存储在 IPFS 上，包含每个动作的 IPFS 散列](https://gateway.ipfs.io/ipfs/QmU7iZuLfeReYxjTopGM6byg2UpABCeQKKdKqjd12UnDct))。因此，用户可以对每 15 个动作收取区块链交易费，而不是对每个动作收取区块链交易费。交易由用户签名，因此是作者身份的证明。

## 区块链交易费拦截器

虽然每批交易费是一个很大的改进，但以下障碍仍然存在:

1.  用户保存交易，然后必须在区块链上等待确认，如果使用安全的低油价，这可能需要一些时间，尤其是在拥堵期间。
    最终结果是用户被阻止发布，直到交易被确认。
2.  高油价可能意味着用户推迟储蓄，直到油价下降。
    最终结果是用户被禁止发帖。
3.  当天然气价格上涨时，使用安全的低天然气价格进行节约可能会导致等待未决交易的时间非常长(数小时或更长)。并非所有签署者/dApp 浏览器目前都支持以更高的油价重新提交交易。(他们最终会这样做)
    最终结果要么是用户被阻止过账，直到汽油价格降低，要么他们以更高的汽油价格重新提交他们的交易(或者提交具有相同现时和更高汽油价格的交易，然后在不支持重新提交的情况下尝试新的交易)

阻止用户发帖给社交网络制造了巨大的障碍，抑制了使用。

## **用户签名动作**

为了避免交易费用的阻碍，用户可以只签署他们的动作和帖子，而不是签署交易。用户可以签署 JSON 或者 JSON 的 IPFS 散列作为作者的证明。dApp 可以对来自所有用户的新动作进行批处理，并将 JSON 存储在 IPFS，将批处理的 IPFS 散列存储在区块链。

dApp 可以定期(例如每小时)在区块链上存储批次。理想情况下，这将尽可能频繁地让其他客户端访问保存的操作和帖子。

在理想情况下，用户会在每个帖子和行为上签名。这意味着其他客户会更快地看到帖子/操作。最终，用户不需要明确地签署每个动作，因为应用程序/签名者可以创建信任来代表他们做这件事([丹·芬利——web 3 UX](https://www.youtube.com/watch?v=j52b3pGUStE))。

短期的解决方案可能会涉及用户签署批量的行动，因为这符合 Peepeth 当前的用户流。尽管对用户来说没有交易费用，但我相信我们会看到用户更频繁地签约，所以批量越来越小。

最终结果是，我们希望从用户进行操作/发布到它被存储在区块链上的时间最短，以便它可以被其他客户端看到。

## 费用

有几个选项可以为此提供资金:

1.  dApp 支付(并通过其他一些货币化方式提供资金)，假设每小时保存一批，那么一批的交易费用大约为每天 1-2 美元。与一些集中托管成本相比，这可能是微不足道的。
2.  用户要么永远支付一次性费用，要么付费订阅。这可以使用令牌来完成。

## 结论

对用户的行为取消区块链交易费消除了执行行为的限制和障碍。每个用户执行的操作数量应该会快速变化。

## 更新

Peepeth 增加了[免费偷窥](https://peepeth.com/a/free)，最初是针对有资格的用户，后来是针对所有人。非常感谢[比万](https://peepeth.com/bevan)peep eth 的创造者提出了这个想法并付诸实践。

## 关于我

我开始使用 Peepeth 是在一个测试网上(感谢@jrmoreau [在 twitter](https://twitter.com/jrmoreau/status/971194600806379521) 上验证)。我是主网上的偷窥者 7 号。

我对去中心化和在移动设备上使用的好处充满热情。