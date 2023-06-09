# 代币经济的激励基础

> 原文：<https://medium.com/coinmonks/incentive-fundamentals-for-a-token-economy-3f161ac04ac4?source=collection_archive---------5----------------------->

一年前，我的职业生涯遭遇了一次严重的挫折，我完全进入了区块链领域。经过几个月的社交和聚会——对一些人来说步行一个半小时，部分是为了更近一步而开始自己的社交和聚会——我很幸运地发现自己能够批判性地思考加密货币，以及哪些因素推动了无许可区块链系统的价值。

这带来了思考激励制度的其他机会，现在我发现自己很高兴与各行各业的初创企业创始人交谈，他们希望将这项技术应用到他们的想法中。

我发现这绝对不仅仅是技术问题，同样也是经济问题。真正的经济学是对人类行为的研究，好的表征模型承认自下而上的经济活动的力量。我花了 23 年时间从事投资管理，为价值因素建模，并从古典自由经济学的角度质疑该行业自上而下的经济教条，这似乎帮助我理解了区块链空间(至少是无许可系统)。然而，不管在哪个领域，这些仍然是由具有盈利动机的个人创办的企业。当我与这些企业家交谈时，他们经常被如何从在区块链做生意中获得经济利益所困扰。

平心而论，加密货币和区块链技术不是由纯粹利他动机的人推动的。他们可能有不同于利润的其他动机，但他们都有从经济上获利的策略。他们意识到有一种不同的方式来创造和获取价值。通过建立激励某些类型工作的网络，他们将在网络上执行的工作的价值货币化——代币或加密货币是他们如何将这种价值货币化的。对于典型的企业家来说，这需要对收入和盈利模式有非常不同的看法。

关于驱动代币价值的生态系统经济学，有两个基本模型在起作用。一个涉及货币数量论，另一个涉及网络价值论。货币数量理论是解释代币内在价值的基本方法。网络价值理论有效地解释了它的总价值，这可以推动基本价值的溢价。

货币数量理论已经存在了几十年，当我开始从事这一行业时，它仍然与分析货币政策相关(尽管在过去三十年里它肯定失宠了)。它指出，总货币供应量(M)乘以货币供应量的年周转率(V ),等于平均商品和服务的价格(P)乘以一年中购买的商品和服务的总量(Q)。因此，MV=PQ。MV 是一段时间内需求的货币总量，PQ 是同一时期内需求的商品总价值。传统上，PQ 是名义 GDP，其中 P 是通货膨胀指数的水平，Q 是实际 GDP。例如，m 是流通中美元供应量的量度，V 是满足购买商品和服务所需的美元年周转率。

现在让我们来看一个令牌，它代表一个存储的或链接到区块链记录的数据单元。在这种令牌的情况下，M 是令牌的总流通供应量(仅发行，不储备或以其他方式锁定不流通)，P 是购买单位数据所需的令牌数量(将其视为系统中单位数据的平均价格)，Q 是一年期间系统上交换的总数据单位，V 是同一时间段内令牌供应量的周转次数。

请注意，P 与令牌值成反比。如果今天一个令牌购买一个单位的数据，那么令牌的持有者可以控制一个单位的数据。如果明天需要两个代币来购买一个单位的数据，那么一个代币的持有者只能支配半个单位的数据，这是价值的贬值。或者，如果明天需要一个代币来购买一个单位的数据，那么一个代币的持有者现在可以控制两个单位的数据，所以他的代币升值了，现在更有价值了。

我们可以重新排列公式，使 P 在等式的一边，因此 MV/Q = P。如果我们的代币升值，P 将减少，因此激励措施应该如下:

1.  保持 M(流通中的总供应量或代币)相对较低
2.  保持 V(年流速)较低
3.  提高 Q(所需的数据总量)

系统内的激励措施应相应调整。标记、燃烧或回收(令牌的循环模型)都有助于最小化 m。增加高价值数据的数量会增加 Q，因此应该有验证质量和贡献数据的机制和/或激励机制—这就是监管模型的作用。速度实际上是对价值增加的预期，如果人们预期价值增加，他们会持有它而不是交换它。如果你把这些事情都做对了，扩大你的人际网络，那么 P 就会下降，token 的价值就会上升。

网络价值理论基于梅特卡夫定律，该定律认为网络的价值与用户数量(U)的平方有关。在梅特卡夫定律的基础上增加了交易的数量，尽管有很多方法来估算交易。我在链接为 [here](/@donyocham/a-network-theory-of-value-model-for-bitcoin-427f78594aaf) 的文章中描述了我对该模型的应用，其中我使用以美元(Tx)计价的每个用户(地址)的令牌交易平均数作为我的交易衡量标准。为了对代币的总市值(代币供应总量*每枚代币的美元价值)或代币市值(MCT)进行建模，我们得到以下结果:

MCT = U^2 * Tx

其含义很简单，增加用户数量(按地址衡量)或每笔交易的美元价值都会推高令牌的价值。网络模型隐含的价值很容易超过系统上数据的内在价值，可以认为是投机价值。无论是对数据未来状态的现值进行定价，还是简单地对交易者之间更大的傻瓜动态进行定价，都是无关紧要的。更多的人交易令牌意味着更多的地址和每个地址更高的美元价值交易，这将推动网络效应更高，即使人为高于链上数据的实际交易价值。

因此，作为数据生态系统本身的创始人，本质上没有收入模式，但有盈利模式。随着时间的推移，出售储备(或创始人持有的)代币，因为代币的美元价格大幅升值，超过了其中数据的价值。

至于收入，系统内应该有很多参与者希望获利，比如数据贡献者、数据分析师、数据做市商等。，他们的收入模式将是赚取代币。他们持有的代币越多，而不是立即兑换成美元，情况就越好(见上文关于速度的讨论)。然后，创始人也可以通过对系统的发展做出贡献来获得收入。

我个人认为，创始人应该避免为了任何单个参与者的利益而直接对该系统征税(这可能被辩护为正式的收入模式)，因为它违反了无许可、开源令牌经济学的原则。货币化是建立正确的激励机制和从象征性升值中获利的结果。我认为收入应该完全来自于供应链上的竞争。否则，这就像是一种特权，代表着对其他参与者的不公平优势，可能会抑制他们的参与。提高代币价值的动机简单明了。

因此，在这种情况下，为了对系统的总价值进行建模，您需要估计您可以在链上获得多少数据、该数据的价值以及数据周转或被需求的次数，因为几个参与者可以要求相同的数据集。然后你估计数据集的增长。鉴于数据市场的规模和数据的价值，这些数字可能会很快变得惊人。如果一个创始人有信心能够精心设计可靠的激励机制，建立强大的用户和贡献者网络，他们可能就拥有了可靠的区块链业务。