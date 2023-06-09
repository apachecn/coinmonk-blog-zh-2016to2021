# 通过令牌管理的注册中心进行学术同行评审*

> 原文：<https://medium.com/coinmonks/scholarly-peer-review-via-token-curated-registries-30e2d2231f0c?source=collection_archive---------0----------------------->

在科学出版中，同行评审过程用于保证特定出版物的质量，无论是期刊文章、科学会议程序还是数据和代码库。

同行评审过程存在许多变体。他们都有相同的目标:帮助评估一项特定的研究是否应该被所讨论的出版物接受。

**同行评审的一些问题**

虽然同行评议在过去已经被用于学术团体的巨大利益，但是同行评议存在几个问题:

1.  **同行评议可能会很贵。运转良好的同行评审过程不容易实施。它们耗费时间和资源。因此，有效的同行评议大多被国际出版社用于高价期刊。**
2.  **同行评议减缓了研究**。从发表到评审一项学术成果的周转时间往往太长。
3.  同行评议让科学话语远离了封闭的牢笼。审稿人的评论通常只发送给编辑和作者。普通读者不会看到它们。
4.  **同行评议并不能很好地记录稿件。审查者的工作没有得到足够的信任，因为他们经常保持匿名，因此他们的输入不能得到足够的信任。**

**令牌管理的注册管理机构作为基于区块链的同行评审系统**

在这篇文章中，我建议将学术评审过程建模为一个令牌管理的注册中心(TCR)。这种以区块链为基础的模式实质上是将审查过程视为一种协议，目的是就一篇给定的研究论文是否应该被发表达成共识。这一过程的基本机制是一个基于区块链的审查过程，由向期刊提交建议的研究人员发起，并由该期刊的令牌持有者执行。

**什么是令牌管理的注册中心？**

在其最基本的形式中，令牌管理的注册表是分散管理的列表，其中所有参与者的经济激励通过精心制作的令牌设计和协议来平衡。TCR 生成一组“消费者”感兴趣的项目列表。名单的所有者是所谓的“代币持有者”，而那些希望被列入名单的人是“候选人”。

**令牌管理的注册管理机构中用户类型的激励**

这三种用户类型都有不同的经济动机，但重要的一点是他们的动机都指向一个共同的目标:创建高质量的注册中心。

**消费者**希望拥有一个高质量的注册表，帮助他们做出决策、学习新知识或有效利用时间，例如不要把注意力浪费在不相关的产品、服务等上。

**候选人**希望获得消费者的关注，因此希望被列入注册。注册质量越高，将被列入的候选人的价值就越高。

**令牌持有者**有通过提供全面的监管服务来产生高质量列表的内在动机。名单的质量越高，就越能得到消费者的关注，也就有越多的候选人希望被列入登记册。这些因素决定了注册中心令牌的价值。

**申请、质询和投票是管理 TCR 的核心机制**

TCR 的中心机制由在列表中加入新条目的申请流程构成。该过程包含以下步骤:

1.  希望被列入登记册的候选人提交申请。作为应用程序的一部分，候选人以注册中心本地令牌的形式(在 TCR 1.0 中。这被称为最小存款参数)
2.  令牌持有者被告知新的应用程序，并且可以在给定的时间范围内查看它(APPLY_STAGE_LEN)。在此期间，可以对列入候选人提出质疑，如果没有人提出质疑，候选人将被列入登记册。
3.  如果申请受到质疑，所有令牌持有者将投票决定是否接纳候选人。这种投票既是令牌加权的，也是秘密的(“提交-展示”)，其想法是，拥有更多令牌的令牌持有者比那些拥有较少令牌的令牌持有者有更高的自然激励来策划高质量的列表，因为令牌重的策展人的公开投票可能会影响其他人的投票。
4.  如果候选人的申请成功，候选人的保证金将归候选人所有，并将候选人变成代币持有者。如果申请被成功质疑，保证金将被没收并分配给对申请提出质疑的人。

如何将 TCR 模型应用于同行评审系统？

TCR 模型中的实体可以很容易地被映射来描述一个简单的同行评审过程。给定同行评审期刊中的实现，映射如下所示:

1.  **令牌持有者**是日志的主要保管人。他们包括杂志的审稿人、编辑和出版商。这些小组由杂志的发起人和过去的作者组成。
2.  候选人是那些希望在杂志上发表新文章的人。作为作者，他们希望通过在杂志上发表自己的作品来获得荣誉和认可。
3.  **消费者**是指那些正在阅读《华尔街日报》的人，或是那些通过查看一位学者对该领域的贡献来审查她资质的人。

**学术同行评议中 TCR 方法的一些秘密经济含义**

1.  TCR 加快了同行评审的速度，因为耗时的强制性讨论和评估只在申请受到质疑的情况下才需要。
2.  在挑战阶段，评论者的评论将在投票后公之于众，并随文章一起发表。如果文章被拒绝，评论将被丢弃或发送给作者。
3.  也是代币持有者的候选人(即已经在《日刊》上公布的候选人)不得投票。他们也没有权利质疑自己的工作。
4.  代币持有者及其身份和代币金额在《华尔街日报》网站上公开列出。根据他们拥有的代币数量，他们可以被指定为出版商、主编、编辑或者仅仅是投稿作者。
5.  在挑战过程完成后，投票结果也会公之于众，以消除部族投票的积极性。
6.  该日志由令牌持有者所有。他们是贡献高质量研究的人，也是成功挑战质量不佳的工作的人。
7.  基础设施的成本可以从候选人的赌注和挑战中被没收的代币中推断出来。通过这种方式，期刊可以在没有第三方补贴的情况下运行。
8.  对于那些不参与审查过程的代币持有者，该系统可以具有任意的代币膨胀，比如 10%。通过这种方式，《华尔街日报》完全由社区运营。
9.  为了确保期刊的广泛和早期采用，第一批令牌可以在感兴趣的学者群体中空投。

> [直接在您的收件箱中获得最佳软件交易](https://coincodecap.com/?utm_source=coinmonks)

[![](img/7c0b3dfdcbfea594cc0ae7d4f9bf6fcb.png)](https://coincodecap.com/?utm_source=coinmonks)

**结论**

令牌管理的注册机构可以成为一个简单的系统，用于建立由社区运营的同行评审期刊，并将产生高质量的投稿。高价期刊订阅可能不会在一夜之间消失，但随着越来越多的高质量期刊在令牌管理的注册中心运行，研究将缓慢但肯定会向公众开放。一个未解决的问题是如何播种初始令牌持有者。要确保注册管理机构的公平和平衡发展，最佳组合是什么？

*感谢 Sven Merten 和 Simon de la Rouviere([@ simondlr](http://twitter.com/simondlr))对本文早期草稿的评论。感谢斯文·默顿让标题变得更好。我要特别感谢 [Trent McConaghy](https://medium.com/u/f1cb98e196bc?source=post_page-----30e2d2231f0c--------------------------------) 让我了解了 TCR，感谢 Mark Gotham 和我一起创办了《计算音乐学杂志》，感谢 Simon de la Rouviere、 [Mike Goldin](https://medium.com/u/4380e912132e?source=post_page-----30e2d2231f0c--------------------------------) ，Sven Merten，Oliver vlkel，Masha 和 [Trent McConaghy](https://medium.com/u/f1cb98e196bc?source=post_page-----30e2d2231f0c--------------------------------) 在符号化方面拓展了我的思维。

TCRs 的概念最早是由 Mike Goldin 在 2017 年 9 月描述的(https://medium . com/@ ilovebagels/token-策展-registries-1-0-61a232f8dac7)。从那以后，该规范得到了进一步的发展([https://medium . com/@ ilovebagels/token-策展的注册表-1-1-2-0-tcrs-new-theory-and-dev-updates-34c 9 f 079 f 33d](/@ilovebagels/token-curated-registries-1-1-2-0-tcrs-new-theory-and-dev-updates-34c9f079f33d))，从而产生了一个用于以太坊应用程序的通用 TCR 存储库([https://github.com/skmgoldin/tcr](https://github.com/skmgoldin/tcr))。