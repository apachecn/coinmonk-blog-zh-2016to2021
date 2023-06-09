# 交易-基本风险管理提示

> 原文：<https://medium.com/coinmonks/essential-risk-management-tips-7ef4cf7e2ef7?source=collection_archive---------5----------------------->

![](img/82fa03a93caa76e118367fd4cf3f513b.png)

交易令人兴奋，尤其是在剧烈波动的加密货币市场。在所有令人兴奋的加密市场中，在没有适当的风险管理的情况下，很容易将谨慎抛之脑后，试图追逐你可能已经看到过的过高回报。然而，这不是明智的投资策略，本质上是赌博。

风险管理是，或者应该是，你整体交易策略的一个关键方面。任何人都可能是幸运的，连续 5 次成功交易；你也可能运气不好，连续输了 5 笔交易。然而，利用适当的风险管理技术可能意味着损失 5 笔交易带来的不便，或者严重耗尽你的可用资本。

我的文章的重点将是使用 [Jesse](https://jesse.trade) 框架用 Python 编写的自动交易系统；然而，你也可以在手动交易中运用这些技巧。

# 资金划拨

在谈论投资时，你最常听到的一句话是“只投资你愿意失去的东西”——这对交易来说也是如此。遵循这个建议将意味着你不太可能做出与交易相关的情绪化决定，并让你在做出财务决策时有一个更好的心态。

在交易之前，你需要决定你想分配给你的策略的**总资本**。交易市场和加密货币市场各自都是高风险企业，合在一起会进一步增加风险。你应该在心理上准备好失去你投入的所有资金，即使这种结果看起来不太可能。这意味着你应该理想地保持你的策略的总资本在你的整个投资组合中占一个低的百分比，确切的数量将取决于你个人的风险承受能力。

# **止损**

止损的使用可以有效地减轻波动市场中的风险，因为它可以让你在不利于你的交易中出局。有许多不同的方法可以用来设置你的止损，其中一些我会在下面展示。

## **静态止损**

止损的一个简单例子是，假设你做多，当资产从你的进场点下跌 5%时卖出。因此，如果你的进场价格是 10000 美元，你的止损将设在 9500 美元。使用 Jesse 框架，这可以表示为:

```
entry = self.pricestop = entry * 0.95
```

在这里，我已经设置了等于资产当前价格的条目，止损设置为 5%。这意味着减少我的损失，以保护我的整体投资组合。显然，我用了 5%的止损作为例子，你需要找出什么对你的策略最有效。更宽的止损意味着更高的潜在损失，但会减少被止损出局的机会，反之亦然。

## **动态止损**

动态止损改善了静态止损，因为它们在设置止损时考虑了当前的市场条件，例如**平均真实范围** (ATR)指标。

ATR 本质上是对给定时期内资产平均波动性的衡量。它对确定止损点很有用，因为它减少了你的交易因市场波动而停止的机会。

本期 ATR 定义为以下各项的最大值:

**当前高—当前低
|当前高—先前收盘|
|当前低—先前收盘|**

这些值在特定时期的平均值被用来形成 ATR 指标。幸运的是，Python 上有很多开源的技术分析库可以为我们计算 ATR。

在这个例子中，我选择了一个 14 周期的 ATR，但是你必须找出什么对你的策略有效。然后你可以用这个值来计算每笔交易的止损:

```
entry = self.pricestop = entry – 2.5 * self.atr
```

在这里，我将止损设置为等于:2.5 倍的 ATR 值。您可能希望在更长的时间范围内(例如 1 天)增加该值，或者在更短的时间范围内(例如 1 小时)减少该值。此外，这是假设你是在一个长期的立场。如果你是空头，你需要将 stop 变量中的符号从-改为+。

## **跟踪止损**

如果资产的走势对你有利，跟踪止损就会移动，但如果资产价格对你不利，跟踪止损就会保持不动。这可以让你在交易中“锁定”你的利润，如果市场对你不利，也会保护你。下面是使用上面显示的 ATR 指标计算的跟踪止损的例子:

# **位置尺寸**

头寸规模是风险管理的另一个重要方面。在每笔交易中冒太大的风险可能会导致在几次糟糕的交易中资本的完全损失。大多数交易者建议拿你资本的 1%来冒险，通常不超过 5%。但是，您可以利用动态头寸规模方法，该方法考虑了各种风险/回报指标，以改善对您的风险敞口的控制，类似于止损示例。

一些专业交易者喜欢的一种方法是凯利标准，这是一个等式，要求盈利交易的历史概率和盈亏率作为输入，并根据这些输入给你一个最佳的头寸规模。凯利标准中使用的公式以及更多细节可在[这里](https://www.investopedia.com/articles/trading/04/091504.asp)找到。这种位置大小技术可以使用 Jesse 框架在 Python 中实现:

所有这些例子都可以用来根据你自己的偏好管理你的风险，通过利用适当的风险管理技术，你可以增加你的策略的盈利能力。

祝市场好运！

**免责声明**:本文提供的信息仅用于教育目的。我不是理财顾问，这篇文章也不包含理财建议。进行自己的研究，做出自己的财务决定，或者咨询专业的财务顾问。

## 另外，阅读

[](/coinmonks/crypto-trading-bot-c2ffce8acb2a) [## 加密交易机器人——最佳免费加密交易机器人

### 2021 年币安、比特币基地、库币和其他密码交易所的最佳密码交易机器人。四进制，位间隙…

medium.com](/coinmonks/crypto-trading-bot-c2ffce8acb2a) [](/coinmonks/best-crypto-signals-telegram-5785cdbc4b2b) [## 最佳 6 个加密交易信号电报通道

### 这是乏味的找到正确的加密交易信号提供商。因此，在本文中，我们将讨论最好的…

medium.com](/coinmonks/best-crypto-signals-telegram-5785cdbc4b2b)