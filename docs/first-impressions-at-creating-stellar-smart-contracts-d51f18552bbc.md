# 创建一流智能合同的第一印象

> 原文：<https://medium.com/coinmonks/first-impressions-at-creating-stellar-smart-contracts-d51f18552bbc?source=collection_archive---------3----------------------->

像许多人一样，我一直很关注加密市场的牛市及其在过去 8 个月中的缓慢下跌。然而，我仍然对此类系统的潜在能力非常感兴趣和着迷。

所以当我在寻找智能契约技能，复习以太坊和稳固性的时候，我偶然发现了恒星智能契约(SCC)。

![](img/9f30f73e120dcbf89387ea4072ff5b06.png)

Henri de Montaut [Public domain], via Wikimedia Commons

以下是我发现的以太坊的一些优点，前提是我要玩的项目是一个简单的项目。

*免责声明:我必须说，这里的重点绝对不是要证明以太坊本身有什么问题，尤其是这些平台的几个方面是设计无法相比的。*

首先，有限的 SCC 能力使得用例更容易理解。简而言之，首先想到的是无边界多货币交易和托管。

考虑到以太坊智能合约被黑客攻击的频率，以及在编写 Solidity 代码，甚至是在已部署的合约上修复代码时，犯错误是多么容易，我发现使用 Stellar 的攻击表面和错误后果更加有限，特别是当价值转移几乎总是涉及到的时候，这是你最不想搞砸的事情。

从用户角度来看，共识的时间效率使其成为许多场景的直接选择。据报道，交易验证大约需要 5s。更好的是，固定和便宜的费用，使事情更简单，更广泛的采用。

另一个因素是 PoW 不与恒星共识协议一起使用，避免了为了信任而燃烧星球资源的耻辱。

# 现在让我们深入一点代码

但是首先(！)，但是有一些先决条件:在您深入研究 SCC 之前，您应该对非对称加密非常熟悉，因为它是理解密钥对用法以及如何处理交易签名的基础。

*免责声明:我很高兴以下(或以上)的内容被证明是错误的，如果需要，请随意评论。*

作为一个新手，我仍然有一些发现和“发现”时刻可以交流，其中之一是关于多重签名(multisig)的情况:当你设置 multisig 时，你在给定的*账户*上设置它，这样任何涉及这个*账户*的交易都需要几个签名人。我必须说，最初，我认为这是一个可以为给定事务定义的属性。我们以后会看到的。

为了方便起见，我们将使用 nodeJS 和 Stellar Javascript SDK，并使用 stellar testnet。

# 简单交易

这个应该很简单，我们接下来要做的是:

*   为 A(lice)和 B(ob)创建种子/公钥对
*   创建并资助具有一流流明的账户
*   创建并提交一个交易，Alice 通过该交易向 Bob 发送 20 XLM

**1。创建密钥对**

所以 Stellar SDK 自带随机密钥生成方式:`Stellar.Keypair.random()`。

在这个代码片段中，创建了 2 对脚本并本地保存在一个文件中，这样我们就可以一个接一个地运行脚本。

**2。向账户添加资金**

一个明星账户必须有一个最低流明数(XLM)才能被平台验证。幸运的是，testnet 有一个 *friendbot* 免费给你 10000 XLM。

所以这个脚本会礼貌地请求 friendbot 通过 http 请求向你的 A(lice)和 B(ob)的 XLMs 帐户提供资金。在现实世界的使用案例中，你必须通过一个锚点来交换你的 fiats 或一个交换平台来添加流明到一个帐户。

脚本执行后，您验证 Alice 和 Bob 的帐户是否有 10K 流明:

注意 balances 是一个数组，因为一个帐户可以有不同的资产(令牌)余额。在我们的简单例子中，我们将使用本地资产 XLM。

**3。创建、签署并提交交易**

以下是关键点:

*   第 13 行，由于平台使用的底层数据类型，XLM 的数量被定义为一个字符串
*   第 23 行，创建该事务是为了将 20 XLM 从爱丽丝发送给鲍勃。设置这个只需要 Alice 和 Bob 的公钥
*   第 31 行，Alice 签署交易(这里需要 Alice 的私钥)。
*   第 33 行，交易被提交给网络。

现在，您可以使用之前的`checkAccounts.js` 脚本来验证修改后的余额。

这是一个非常基本的用例，在下一篇文章中，我们将创建一个托管合同，Alice 和 Bob 都必须签字才能释放资金。

你还会发现我在这个频道发布的几个视频:[https://www.youtube.com/channel/UCAKSRclAwfHyN6dWibj4iVw](https://www.youtube.com/channel/UCAKSRclAwfHyN6dWibj4iVw)

*更新(27/08/2018):下一篇，* [*简单代管合同使用恒星*](/@slyg/simple-escrow-contract-using-stellar-67aa799f7db) *。
更新(26/06/2019):在 sdk 更新后更新代码示例。
更新(20/08/2019):更新 sdk 更新后的代码示例，添加视频链接*

> [直接在您的收件箱中获得最佳软件交易](https://coincodecap.com/?utm_source=coinmonks)

[![](img/7c0b3dfdcbfea594cc0ae7d4f9bf6fcb.png)](https://coincodecap.com/?utm_source=coinmonks)