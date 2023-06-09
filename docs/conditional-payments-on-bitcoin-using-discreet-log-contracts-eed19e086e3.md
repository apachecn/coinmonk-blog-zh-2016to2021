# 使用谨慎日志合同的比特币有条件支付

> 原文：<https://medium.com/coinmonks/conditional-payments-on-bitcoin-using-discreet-log-contracts-eed19e086e3?source=collection_archive---------4----------------------->

Discreet log contract 是一种有条件的支付机制，由比特币闪电网络论文的合著者、麻省理工学院数字货币倡议团队的成员 Tadge Dryja 发明。[论文](https://adiabat.github.io/dlc.pdf)写于 2017 年。

![](img/93e59bc5d07dada459ddbf70c0219ea4.png)

# 概观

目前，像比特币和以太坊这样的区块链上的有条件支付是通过“智能合约”完成的，其中的条件是用比特币脚本这样的限制语言或 Solidity 这样的图灵完整语言编码的，这些代码部署在 chain 上。该链稍后将执行该代码以及所提供的其他输入，然后将资金转移到其中一个参与者。这需要链知道条件，因此世界上的任何人都可以了解条件。此外，该链必须执行这些条件，从而耗费资源。谨慎日志合同旨在通过不将这些条件放在链上来解决这些问题，而只需要来自预先决定的公钥的签名来决定将资金转移到哪里。这个预先确定的密钥可以是一个神谕，该神谕被期望诚实地行动并在正确的消息上发布签名。先知不必知道合同的细节。它也不必在区块链上公布它的签名。

谨慎日志合同适用于两个互不信任的方对公共事件(如体育赛事、选举或商品或货币的价格)的结果进行打赌的情况。在这里，体育赛事的广播频道、新闻频道或股票市场信息将充当先知。先知发布的签名将出现在作为事件结果的消息上。

谨慎的日志合同需要 Schnorr 签名，但 Schnorr 签名不会在链上发生，因此它们今天可以与比特币一起使用。该签名是离线生成的，并且该签名揭示了一个秘密，该秘密将用于创建一个私钥，该私钥将最终控制资金。

# 预赛

1.  施诺尔签名。谨慎日志契约的关键思想是基于 Schnorr 签名。它是这样工作的(使用 ellpitic curves):
    *(I)*`*G*`*`*h*`*是协议参数(因此大家都知道)。* `*G*` *是群的生成器* `*h*` *是散列函数。
    (ii)签名者选择秘密密钥* `*x*` *并计算公开密钥* `*X*` *。* `*X = xG*` *。
    (iii)为了对消息* `*m*` *进行签名，签名者生成秘密随机数* `*k*` *并发布对随机数的承诺，比如* `*R*` *。这里* `*R = kG*` *。* `*R*` *叫做承诺。
    (iv)签名者将消息和公共随机数散列在一起以创建挑战* `*c*` *。* `*c = h(m||R)*` *(v)签名人然后计算* `*s = k - cx*` *。****m 上的签名是*** `***(R, s)***` *。* `*s*` *称为响应(对挑战)
    (六)验证签名，计算* `*sG*` *并检查是否等于* `*R - cX*` *。此作品自* `*sG = (k - cx)G = kG - cxG = R - cX*` *开始。**
2.  ***多签名输出**。比特币交易产生输出并花费输入(以前未花费的输出)，其中每个输出通常由一个私钥控制。多重签名输出是由一个以上的私钥控制的输出，因此要使用这样的输出，需要两个私钥。(不完全正确，可能有不同的组合，如 2 选 1，需要 2 个私钥中的 1 个来控制输出，3 选 2，需要 3 个私钥中的 2 个，以及[等等](https://en.bitcoin.it/wiki/Multisignature)。*
3.  ***时间锁定输出**:比特币脚本允许创建交易，其中输出在特定时间(准确地说是特定块高度)之前不能被花费。这种时间锁可以与其他条件相结合，例如该输出只能在任何时候由地址`A`使用，或者在特定时间后由地址`B`使用。*

# *谨慎的日志合同*

*在谨慎的日志契约中，oracle 使用 Schnorr 签名，但方式不同。甲骨文在决定签署什么消息之前就发布了签名，而不是在签名中发布承诺。假设先知打算公布他对未来将要发生的事件结果的签名。它有一个长期公钥`X`，它选择一个随机数秘密随机数`k`，计算对随机数的承诺为`R` (= `kG`)并发布`R`。(`X`，`R`)是一次性公钥。**现在，如果有人知道签名，他可以创建一个“特殊的公钥”，** `**Y**` **(因此一个比特币地址)涉及签名，其私钥只有他知道。**因此，如果有人期望甲骨文在消息`b`上发布签名，他可以使用他的私有-公共密钥`p`和`P` (= `pG`)并创建`Y`作为
(I)`sG = R - h(b, R)X`
【ii】`Y = P + sG`
当甲骨文发布签名`s`时，`P`的所有者将创建知道`Y`的私有密钥，因为`Y = P + sG = pG + sG = (p + s)G`。在甲骨文公布签名之前转移到`Y`的任何资金都可以由`P`的所有者认领。这是谨慎日志契约的关键思想。*

***示例***

1.  *拥有公钥`A`的爱丽丝和拥有公钥`B`的鲍勃决定对一个事件(如梅威瑟&帕奎奥之间的拳击比赛)下注，该事件的结果可以是`a`(梅威瑟赢)或`b`(帕奎奥赢)，爱丽丝选择`a`，鲍勃选择`b`。在实践中，`a`和`b`应该被随机化以避免重放攻击(梅威瑟&帕奎奥之间可能有多场比赛，应该清楚赌哪场比赛)。*
2.  *Alice 和 Bob 就一个神谕达成一致，该神谕将在稍后的某个时间点通过`a`或`b`发布签名。oracle 有一个长期密钥对和一个一次性密钥对(仅用于此事件)。一次性密钥对是 Schnorr 签名中的承诺步骤。*
3.  *神谕公布了承诺`R`。*
4.  *Alice 和 Bob 创建(但还没有在区块链上写)一个多签名事务，比如说`F`，该事务由他们共同出资，并创建一个 2/2 多签名输出。他们交换交易 id(没有签名)。该交易被称为**合同资金交易**。*
5.  *爱丽丝和鲍勃现在都创建了一个交易，他们将资金交易`F`中的钱转移到他们的“特殊公钥”中。这个“特殊的公钥”是在假定 oracle 在他们的首选结果上发布签名的情况下生成的。因此，爱丽丝的特殊公钥 Aₐ将假设甲骨文将在`a`发布签名，鲍勃的特殊公钥`B_b`将假设甲骨文将在`b`发布签名。
    请注意，有两种以上的可能结果，Alice 和 Bob 将创建更多这样的交易。这些交易被称为**合同执行交易**。
    *爱丽丝创建“特殊公钥”一个*ₐ
    *(I)*`*s*ₐ*G = R - h(a, R)X*` *(ii)*`*A*ₐ *= A + s_aG*` *鲍勃创建“特殊公钥”*`*B_b*` *(I)*`*s_bG = R - h(b, R)X*`
    (ii)`*B_b = B + s_bG*`*
6.  *Alice 和 Bob 都将创建他们的合同执行事务，使得输出是一个脚本，该脚本允许事务的创建者在知道私钥的情况下立即得到支付，但是在 timelock 之后，另一方可以得到它。因此，Alice 将创建一个事务，这样，如果他知道私钥，他的特殊密钥`A_a`可以使用输出，但是如果他在特定时间内没有使用它，Bob 可以。对鲍勃来说反之亦然。这样做是为了防止任何一方提交具有无效输出的交易并锁定资金。例如，如果 oracle 在`a`上发布签名，那么 Bob 失败了，现在 Bob 仍然可以将资金交易`F`用于输出`B_b`。在这种情况下，Bob 不能花费输出，但 Alice 也不能。为了防止这种情况，合同执行事务的输出有一个可选的时间锁。*
7.  *Alice 和 Bob 现在交换他们的合同执行事务和签名。*
8.  *爱丽丝和鲍勃现在在资金交易`F`上交换签名，然后在区块链上发布`F`。Alice 和 Bob 在签署`F`之前签署并交换他们的合同执行事务是至关重要的。*
9.  *现在，当 oracle 发布签名的结果时，获胜者可以通过在区块链上发布相应的合同执行事务并立即将其合同执行事务的输出花费到完全由他控制的地址来要求资金。*

*谨慎的日志合同是一个有趣的想法。虽然需要信任一个神谕听起来像是一种限制，但这种信任可以在几个神谕之间扩散。这可以通过不要求仅仅一个先知发布签名，而是几个来实现。例如，Alice 和 Bob 可以决定 3 个新闻频道(作为先知),并同意 3 个新闻频道中的 2 个频道同意一个结果，该结果被认为是可接受的。Schnorr 签名在这里很有帮助，因为它们可以用来构造阈值签名。每个 oracle 都有自己的私有-公共密钥对。然后，先知们将参与一个交互式协议来发布承诺，并随后发布签名。*

*这是来自 DCI github 的谨慎日志合同的规格。*

> *[直接在您的收件箱中获得最佳软件交易](https://coincodecap.com/?utm_source=coinmonks)*

*[![](img/7c0b3dfdcbfea594cc0ae7d4f9bf6fcb.png)](https://coincodecap.com/?utm_source=coinmonks)*