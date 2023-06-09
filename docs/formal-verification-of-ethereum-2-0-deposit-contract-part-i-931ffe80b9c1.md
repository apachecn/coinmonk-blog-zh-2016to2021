# 以太坊 2.0 存款合同的形式化验证(上)

> 原文：<https://medium.com/coinmonks/formal-verification-of-ethereum-2-0-deposit-contract-part-i-931ffe80b9c1?source=collection_archive---------0----------------------->

## 在 Daejun 公园旁边

![](img/3f2f849977bfbd5075d3f04fec4d492e.png)

以太坊 2.0 来了。而且放心，会正式指定验证的！

[以太坊 2.0](https://github.com/ethereum/eth2.0-specs/blob/master/specs/core/0_beacon-chain.md) 是一种新的分片 PoS 协议，在其早期阶段(称为阶段 0)，与现有的 PoW 链(称为 Eth1 链)并行存在。虽然 Eth1 链由矿工驱动，但新的 PoS 链(称为信标链)将由验证器驱动。

在信标链中，验证器的作用是创建(称为建议)和验证(称为证明)新块。信标链的共识协议建立在重要的小工具之上，[卡斯珀·FFG](https://arxiv.org/abs/1710.09437)用于最终确定区块，[LMD·幽灵](https://vitalik.ca/general/2018/12/05/cbc_casper.html)用于叉子选择规则，RANDAO 用于生成随机数。(查看[我们之前的约定](https://runtimeverification.com/blog/runtime-verification-completes-formal-verification-of-ethereum-casper-protocol/)卡斯珀 FFG 的[形式验证和冉道](https://github.com/runtimeverification/casper-proofs)的[统计验证。2)共识协议保证(在某些温和的假设下)期望的安全性和活性属性，只要大多数验证者在创建和验证新块时忠实地遵循该协议。](https://github.com/runtimeverification/rdao-smc)

验证器集是动态的，这意味着新的验证器可以加入，现有的验证器可以随着时间的推移退出。要成为(新的)验证者，需要通过向指定的智能合约(称为存款合约)发送交易(通过 Eth1 网络)来存款一定量的以太作为“股份”。存款合同记录存款的历史，信标链检索该历史以维护动态验证器集。(不过，这种存款过程将在稍后阶段发生变化。)

![](img/04cd4d75f3ee50fa1f207039d8c69c09.png)

## 存款合同

用 [Vyper](https://vyper.readthedocs.io/) 编写的[存款契约](https://github.com/ethereum/deposit_contract)采用 [Merkle 树](https://en.wikipedia.org/wiki/Merkle_tree)数据结构来有效地存储存款历史，其中每当接收到新存款时，该树就被动态*更新(即，从左到右依次递增地添加叶节点)。这里，合同中使用的 Merkle 树预计非常大。事实上，高度为`32`的 Merkle 树可以存储多达`2^32`个存款，在合同的当前版本[中实现。由于 Merkle 树的大小是巨大的，所以每次接收到新的存放时重建整个树是不实际的。](https://github.com/ethereum/eth2.0-specs/blob/12293a91b4333bf419eea7e6b897dd5f988f9e23/deposit_contract/contracts/validator_registration.v.py)*

为了减少时间和空间需求，从而节省天然气成本，合同实施了[增量 Merkle 树算法](https://github.com/ethereum/research/blob/master/beacon_chain_impl/progressive_merkle_tree.py)。增量算法享受`O(h)`时间和空间复杂度来重建(更准确地说，计算根)高度`h`的 Merkle 树，而朴素算法将需要`O(2^h)`时间或空间复杂度。具体来说，该算法维护两个长度为`h`的数组，更新树的每次重构只需要计算从新叶(即新存放处)开始到根的链，其中链的计算只需要这两个数组，从而在树的高度上实现*线性*时间和空间复杂度。

然而，有效的增量算法导致存款合同实现不直观，并使得确保其正确性变得不容易。考虑到存款合同的极端重要性，需要进行正式验证，这是最终保证合同正确性的唯一已知方法。

![](img/d6a66fcaccc8a50153b870318c408feb.png)

## 增量 Merkle 树算法的形式化验证

[在以太坊基金会](https://blog.ethereum.org/2019/05/21/ethereum-foundation-spring-2019-update/)的资助下，我们在[运行时验证](https://runtimeverification.com/)开始了存款合同的正式验证，今天我们很高兴地宣布我们实现了第一个里程碑*增量 Merkle 树算法的正式验证*。

具体来说，我们首先*严格形式化*增量 Merkle 树算法。然后，我们提取了存款合同中使用的算法的伪代码实现，并且*正式证明了*伪代码实现的正确性。这意味着存款契约在源代码级别是正确的，也就是说，它将正确地增量计算 Merkle 树根，前提是没有由 Vyper 编译器引入的编译时错误或 EVM 字节码级别的功能正确性错误。(事实上，我们的下一个任务是确保这种字节码级的正确性。)查看 [***我们的报告***](https://github.com/runtimeverification/verified-smart-contracts/blob/master/deposit/formal-incremental-merkle-tree-algorithm.pdf) 进行算法的形式化和正确性证明。

## 调查的结果

在我们的形式化和正确性证明工作过程中，我们发现了存款合同的一个[微妙的 bug](https://github.com/ethereum/deposit_contract/issues/26) ，在最新版本中已经[修复了](https://github.com/ethereum/deposit_contract/commit/e357f3797bd37116564fb1e41605864d6fffb45f)，以及几个[重构建议](https://github.com/ethereum/deposit_contract/issues?q=is%3Aissue+author%3Adaejunpark)，可以提高代码可读性，降低汽油成本。

让我们详细说明这个微妙的错误。在我们被要求验证的契约版本中，当 Merkle 树的所有叶节点都填充了存款数据时，就会触发错误，在这种情况下，契约(具体来说就是`get_deposit_root`函数)会错误地计算树的根哈希，从而返回零根哈希(即空 Merkle 树的根哈希)，而不管叶节点的内容如何。例如，假设我们有一棵高度为 2 的 Merkle 树，它有四个叶节点，每个叶节点都填充了某些存款数据，分别为`D1`、`D2`、`D3`和`D4`。然后，当树的正确根哈希是`hash(hash(D1,D2),hash(D3,D4))`时，`get_deposit_root`函数返回`hash(hash(0,0),hash(0,0))`，这是不正确的。

由于代码的复杂逻辑，在不大量重写代码的情况下正确修复这个 bug 并不容易，因此我们建议了一个简单的解决方法，强制永远不填充最后一个叶节点，即最多只接受`2^h - 1`存放，其中`h`是树的高度。然而，我们注意到，在当前设置中触发这种错误行为是不可行的，因为最小存放量是 1 个乙醚，并且乙醚的总供应量小于比`2^32`小得多的`130M`，因此填充高度为 32 的树的所有叶子是不可行的。然而，正如我们所建议的，这个 bug 已经被契约开发者[修复了](https://github.com/ethereum/deposit_contract/commit/e357f3797bd37116564fb1e41605864d6fffb45f),因为契约可能被用在其他设置中，在这些设置中，可能会触发 bug 行为并且有可能被利用。更多详情请参见此处的[和](https://github.com/ethereum/deposit_contract/issues/26)。

我们还想指出，这个 bug 很难捕捉。事实上，我们最初认为原始代码是正确的，直到我们写下正确性定理的正式证明时陷入困境。我们最初试图证明正确性的失败导致我们确定了一个定理成立所需的缺失前提(即，一个附加的先决条件)，从中我们可以找到上面的错误行为场景，并提出了[错误修复](https://github.com/ethereum/deposit_contract/commit/e357f3797bd37116564fb1e41605864d6fffb45f#diff-0b42db22e6da8cca278c93c388249c68R78)。这一经验再次证实了形式验证的重要性。虽然我们并不“幸运”地在审查代码时发现了这个 bug，这是所有传统安全审计员都会做的，但是正式的验证过程*彻底地*引导甚至“迫使”我们最终找到了它。

## 下一步是什么？

因此，现在我们确信增量 Merkle 树算法及其存款契约的实现在源代码级别是正确的，我们正在前进，正式验证其编译的 EVM 字节码的行为是否符合预期。我们将在故事的第二部分完成后发布。敬请期待！

## 感谢

我们要衷心感谢以太坊基金会对这项工作的资助，以及丹尼·瑞安、卡尔·贝克休斯和林暐的讨论和及时解决报告的问题。

> [在您的收件箱中直接获得最佳软件交易](https://coincodecap.com/?utm_source=coinmonks)

[![](img/7c0b3dfdcbfea594cc0ae7d4f9bf6fcb.png)](https://coincodecap.com/?utm_source=coinmonks)

*原载于 2019 年 6 月 12 日*[*https://runtimeverification.com*](https://runtimeverification.com/blog/formal-verification-of-ethereum-2-0-deposit-contract-part-1/)*。*