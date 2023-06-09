# AAVE 如何以最漂亮的方式黑掉 ERC-20 代币

> 原文：<https://medium.com/coinmonks/how-aave-hacked-the-erc-20-token-in-the-most-beautiful-way-3e04fb4410fb?source=collection_archive---------2----------------------->

![](img/84eb503417b662f09fa071ddd0930df9.png)

Source: [Aave.com](https://aave.com/branding/).

> "在没有交易的情况下，每个块的代币余额如何增加？"

这个问题让我深入剖析了 AAVE 智能合约，并让我明白...良好的...感觉有点傻。这将是一个技术解释，AAVE 如何在幕后工作，给你实时更新 LP 令牌余额，以及如何可以很容易地被恶意或不安全的方式使用。最后，您应该知道如何在您的 ERC-20 合同中安全地实现相同的功能。

我写这篇文章，不是为了给不喜欢重新发明轮子的开发人员提供一步一步的指导(尽管也可能是这样)，而是为了向 AAVE 团队的惊人工程致敬。我与他们没有任何关系，尽管我在自己的工作中与他们的合同有过互动。他们的实时更新 LP 令牌(aka: aToken)提供了流畅、方便、全面的愉快用户体验。谁不喜欢看着自己账户上的余额在眼前上升呢？

在这一点上，我应该停下来澄清一下:我不确定这种计算收益的令牌结构是从哪里产生的。我第一次发现它是在 AAVE 的合同集中，尽管这个想法可能是从另一个人那里借来的。如果是这种情况，请留下评论，以便信贷可以适当地归因。

介绍完毕。开始潜水:

![](img/b5bf6f661ceabe9a1be859df1ddb7bc9.png)

Excerpt from AAVE’s documentation — ERC20 interface

我们将从 dApp 开发者字典中最常被调用的两个函数开始:balanceOf()和 totalSupply()。AAVE 在这里解释说，调用这些函数是为了分别返回“……拥有的代币数量……”和“……存在的代币数量……”。对于我们这些习惯了这些的人来说，没有什么大惊小怪的，医生也没有显示任何异常。

如果 AAVE 的 aToken 在每次调用 balanceOf()时都返回一个不断递增的数字，那么是谁在支付汽油费呢？有些东西没有加起来(双关语)。

为了更好地理解，下面是 AAVE 通过 GitHub 获得的实际 aToken 合同(v2)的摘录:

![](img/0b73c590207bcdc21698fa97e038c13b.png)

Source: [https://github.com/aave/protocol-v2/blob/master/contracts/protocol/tokenization/AToken.sol](https://github.com/aave/protocol-v2/blob/master/contracts/protocol/tokenization/AToken.sol)

乍一看，我们看到的是同一个 balanceOf()函数，用法和以前一样。仔细观察，这里发生了更多的事情。我将首先强调“override()”方法的使用。这阻止了协定使用标准的 ERC20 balanceOf()逻辑，而是在调用时执行该函数。

> “好吧，酷。尽管函数调用是相同的，但逻辑是不同的。它如何在不消耗汽油的情况下返回一个动态值？”

也许之前的问题是个谎言。这才是真正让我绞尽脑汁的问题:一个函数调用，是只读的，怎么可能每个块都返回不同的值？要回答这个问题，首先我们需要理解 AAVE 计算借贷利率的指数系统(APY 和 API)。

每当资产池更新时，无论是新的流动性存款、贷款偿还还是抵押品清算，都会重新计算这一“流动性指数”。从功能上来说，这其实是以时间作为考虑因素来计算的。毕竟 APY 和 API 都是*年度*计算。这方面的细节不在本文的分析范围内，但是要彻底了解，除了交易之外，时间是保持指数增长的另一个因素。

因此，流动性指数是 AAVE 贷款人和借款人与其相对流动性池关系的活生生的代表。为了理解流动性指数是如何用于计算阿托肯余额的，我们需要理解阿托肯合同中的 scaledBalanceOf()函数，AAVE 在这里解释道:[https://docs . aave . com/developers/the-core-protocol/阿托肯#scaledbalanceof](https://docs.aave.com/developers/the-core-protocol/atokens#scaledbalanceof)

简而言之，当存款时，它被存储为用户的“比例余额”，这意味着作为存款价值和存款时 AAVE 池市场的表示。因为我不擅长高等代数，所以我会按照我的速度来分解它:

> 存款金额/当前流动性指数=比例余额

现在我们有了存储在 AAVE 合约中的用户余额的实际值，我们如何找到任意给定时间的当前余额呢？只需颠倒公式:

> 缩放余额*当前流动性指数=原始余额

因为流动性指数只会增加，所以如果不提款，账户的总余额永远不会下降。这使得整个索引系统是单向的，因此更加简单和安全。

> “无气计算解释，求求！”

既然所有的部分都在这里，让我们把它们组合起来。您可能已经在 aToken 契约的 balanceOf()函数的快照中注意到了，该函数的范围被限制为“view”(正如余额检查函数应该的那样)。有时我会忘记，仅仅因为一个函数被限制为只能查看，它仍然可以计算复杂的算术，如果需要的话可以进行跨契约调用，只要它不用于更新状态。嗯，这就是正在发生的事情。

为了获得最近的余额，aToken 合约必须计算当前的*流动性指数乘以“super.balanceOf”(引用基础激励性 C20 合约的 balanceOf())，这也是调整后的余额。一旦返回了索引(同样，通过只读计算)，不存储它，就可以检索帐户的当前余额。*

这就是了。AAVE 如何黑掉我们对 ERC 的理解——20 个代币做出惊人的东西！

> "函数重写可能存在哪些安全风险？"

现在我们知道了它是如何做到的，我们可以看到它不仅可以用来使 ERC20 交互更加用户友好，还可以看到假设任何名为 balanceOf()、transfer()或任何其他标准的函数的安全性的危险。总是检查合同的完整性，而不是盲目地打电话。

另一个可能的问题是算法——如果 AAVE 的合同为用户产生了比他们实际应得的更多的 aTokens 怎么办？使用该模型，我们需要将影响用户余额的所有因素纳入流动性指数计算中。这就是为什么在任何合同公开之前都应该进行彻底的测试。

> 加入 Coinmonks [电报频道](https://t.me/coincodecap)和 [Youtube 频道](https://www.youtube.com/c/coinmonks/videos)了解加密交易和投资

## 也阅读

[](/coinmonks/leveraged-token-3f5257808b22) [## 杠杆代币[多头代币]终极指南

### 杠杆化令牌是具有杠杆化风险敞口的 ERC20 令牌，不考虑保证金、要求、管理…

medium.com](/coinmonks/leveraged-token-3f5257808b22) [](https://blog.coincodecap.com/crypto-exchange) [## 最佳加密交易所| 2021 年十大加密货币交易所

### 编辑描述

blog.coincodecap.com](https://blog.coincodecap.com/crypto-exchange) [](https://blog.coincodecap.com/best-swap-platforms) [## 2021 年最佳加密交换平台| CoinCodeCap

### 如果我们看看今天的场景，许多加密货币交换平台提供了广泛的功能和深度…

blog.coincodecap.com](https://blog.coincodecap.com/best-swap-platforms) [](/coinmonks/top-5-crypto-lending-platforms-in-2020-that-you-need-to-know-a1b675cec3fa) [## 2021 年最佳加密借贷平台| 6 大比特币借贷平台

### 获得比特币和其他加密货币的最佳贷款利率

medium.com](/coinmonks/top-5-crypto-lending-platforms-in-2020-that-you-need-to-know-a1b675cec3fa) [](/coinmonks/the-best-cryptocurrency-hardware-wallets-of-2020-e28b1c124069) [## 2021 年 6 大最佳硬件钱包|顶级加密硬件钱包[更新]

### 最好的加密货币硬件钱包是绝对必要的。我们将在 NGRAVE、Ledger Nano X 和…

medium.com](/coinmonks/the-best-cryptocurrency-hardware-wallets-of-2020-e28b1c124069) [](/coinmonks/crypto-trading-bot-c2ffce8acb2a) [## 2021 年最佳免费加密交易机器人

### 2021 年币安、比特币基地、库币和其他密码交易所的最佳密码交易机器人。四进制，位间隙…

medium.com](/coinmonks/crypto-trading-bot-c2ffce8acb2a)