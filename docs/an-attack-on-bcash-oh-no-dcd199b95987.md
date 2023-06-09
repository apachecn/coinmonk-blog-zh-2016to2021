# 对#BCash 的攻击？！哦不…

> 原文：<https://medium.com/coinmonks/an-attack-on-bcash-oh-no-dcd199b95987?source=collection_archive---------7----------------------->

之前攻击#比特币及其#LightningNetwork 的神秘组织*又回来了。他们以#BitPico 的名字运营，是一个以试图利用漏洞而闻名的匿名实体。这一次，他们声称正在研究可能“导致多个分叉”的#BitcoinCash 攻击。我不知道这种说法实际上是否可行，但肯定不会反对看到它发生。在去年的分裂狂潮中，他们作为#Segwit2x 的支持者首次曝光，所以看起来他们已经改变了主意。通读一篇旧的#LinuxFoundation [帖子，](https://lists.linuxfoundation.org/pipermail/bitcoin-segwit2x/2017-November/000706.html)以下是支持更大块的内容:*

![](img/c4f06064d0cab8fb6ac476a9f0980e98.png)

而在[的另一个](https://lists.linuxfoundation.org/pipermail/bitcoin-segwit2x/2017-November/000689.html):

![](img/1b67b888b40de1c148f2387bca088a8f.png)

我们知道，那次袭击没有成功。要么是他们的哈希力量对分裂没有足够强的影响，要么是他们因为缺乏共识而放弃了。不知道该责备谁，也不知道现在这是否重要，但这确实意味着他们最近的断言应该持保留态度。这篇[副新闻](https://medium.com/u/cc93e470890d?source=post_page-----dcd199b95987--------------------------------) [文章](https://motherboard.vice.com/en_us/article/evbkza/bitcoin-2x-fork-bitpico-forking-anyway)突出了这个未知实体声称控制的散列功率量的危险信号。过了一段时间，他们[承认了](https://lists.linuxfoundation.org/pipermail/bitcoin-segwit2x/2017-November/000689.html):

![](img/daa61db9cac66b031c961682dfdffad9.png)

相反，他们的#LightningNetwork 压力测试进行得很顺利，似乎没有恶意。开发者 Alex Bosworth 在这条[推特](https://twitter.com/alexbosworth/status/978069194385252352)中提到“闪电剂量器看起来有组织有动机”。根据这篇 [CoinDesk](https://medium.com/u/f2fa6f2d51a6?source=post_page-----dcd199b95987--------------------------------) [文章](https://www.coindesk.com/bitcoins-lightning-network-attacked-good/)，这些攻击都与安全有关:

*“作为投资比特币的人，我们希望确保第二层解决方案不会被淘汰；尝试尽可能多的攻击是唯一确保的方法。”*

![](img/dae0857cef1cd31629211afe6a13b61f.png)

不管怎样，众所周知的咒语是正确的:[不要相信，核实](https://twitter.com/bitPico/status/980978688740163584)。现在他们声称他们已经“为#Bcash 重新装备了他们的 LN 压力测试套件。”我相信我们社区中的许多人都祈祷这是真的。

在一封泄露的与 CoinDesk 的 [Alyssa Hertig](https://medium.com/u/aa1cb20d48d3?source=post_page-----dcd199b95987--------------------------------) 的电子邮件交流中，他们为他们的下一次攻击奠定了一些基础。

她问道:“你为什么攻击比特币现金？”
回应:“这是代表投资者验证比特币现金网络完整性的压力测试。在过去，比特币受到了好的或坏的攻击，但它的完整性仍然完好无损，这为投资者提供了信心。所有分散式计算机网络的第一条基本规则是，如果它没有受到足够的攻击来解决任何问题，那么它的结局将会很糟糕。”

然后:“你在执行什么样的攻击？”
回应:“从低级别的 TCP/IP 栈攻击到高级别的比特币现金协议攻击，无所不包。这种组合保证了几件事:1 .运行在 VPS 上的最弱节点将崩溃或耗尽带宽，并变得无响应。2.所有其他节点将开始从多个背对背预挖掘的 32 兆字节数据块中停止。我们的 LevelDB 压力测试显示，复杂的 32 兆字节块上的比特币现金 UTXO 数据库可能需要高达 200 的 RAM 才能完全处理，如果该 RAM 不可用，UTXO 数据库将会损坏，如果 LevelDB 不释放内存，操作系统将变得无响应。”

最后一个问题是:“你为什么认为你能叉开区块链？”
回应:“我们相信我们已经完善了我们的方法，我们的研究是结论性的；LevelDB 无法支撑我们复杂的区块，比特币现金 85%集中在少数数据中心和矿工手中。如果比特币现金 15%集中，那么我们的努力将会失败，除了 LevelDB 的弱点；我们一如既往地充满信心。当我们攻击比特币 LN 时，我们全面失败，除了大量崩溃的节点，但它们很快被修复并再次运行。这对比特币 LN 来说是双赢的；只有时间能告诉我们比特币现金将如何经受住冲击。”

这对#BCash 意味着什么？很可能什么都没有，如果他们的说法是未经证实的。不管怎样，还是继续一厢情愿吧。如果罗杰·德欺诈性的希望和梦想被遗忘，那将是我们所有人的福音…

这里是我找到他们的#LinuxFoundation 帖子的地方:
[https://lists . Linux foundation . org/piper mail/bit coin-seg wit 2x/2017-11 月/](https://lists.linuxfoundation.org/pipermail/bitcoin-segwit2x/2017-November/) < -点击链接，ctrl+f“bit pico”

更多信息请在推特上关注我:[https://twitter.com/coop__soup](https://twitter.com/coop__soup)