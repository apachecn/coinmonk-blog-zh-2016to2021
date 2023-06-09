# 为您的 Dapps 削减汽油成本的技巧

> 原文：<https://medium.com/coinmonks/techniques-to-cut-gas-costs-for-your-dapps-7e8628c56fc9?source=collection_archive---------0----------------------->

从长远来看，成本对每个基于以太坊的 Dapp 都起着重要作用。我们分享了一些实践技巧，例如，帮助您的 Dapps 削减相关的天然气成本。如果您有任何反馈，我们非常乐意倾听。

**我们在文章中使用的术语**:

*   SC:智能合约
*   GC:天然气成本
*   SOLC:可靠性编译器
*   EVM:以太坊虚拟机

**整体方法** : *基于反馈的解决方案。*

![](img/a6b616b727b6bbd25761abe8ea814bda.png)

我们试图按照 SOLC 和 EVM 的期望优化我们供应链的设计和实施。我们通过执行 GC 回归测试来验证这些想法，并将反馈应用到设计和实现中。

**技术**我们用来削减天然气成本(*注意，出于讨论目的，我们使用 SOLC 版本 0 . 4 . 21)*:

*   ***位压缩***

外部函数参数的位压缩格式有助于削减天然气成本，因为它最大限度地减少了发送到以太坊区块链的输入数据量。请注意，它会引入一些额外的气体成本来解包数据。然而，节省的成本通常会抵消额外的成本。

以下面的代码片段为例:

```
*pragma solidity ^0.4.21;**contract bitCompaction {
  function oldExam(uint64 a, uint64 b, uint64 c, uint64 d) public {
  }
  function newExam(uint256 packed) public {
  }
}*
```

其对应的汇编代码显示，oldExam 函数一旦被调用就会引发 4 个` *CALLDATALOAD`* 操作，每个` CALLDATALOAD '操作都会触发以太坊中的一次内存分配操作，而 newExam 函数只会引发 1 次。请注意，汇编代码太大，我们无法在此展示，但是可以通过执行(例如)“solc-ASM-optimize-optimize-runs 200 bit compact . sol ”,轻松生成它。

在实验中，我们有每个函数的以下调用数据:

```
oldExam call data: ([]uint8) (len=132 cap=132) {
00000000  3e f2 62 fd 00 00 00 00  00 00 00 00 00 00 00 00
00000010  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
00000020  00 00 00 01 00 00 00 00  00 00 00 00 00 00 00 00
00000030  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
00000040  00 00 00 01 00 00 00 00  00 00 00 00 00 00 00 00
00000050  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
00000060  00 00 00 01 00 00 00 00  00 00 00 00 00 00 00 00
00000070  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
00000080  00 00 00 01
}newExam call data: ([]uint8) (len=36 cap=36) {
00000000  83 ba 6e 5a 00 00 00 00  00 00 00 01 00 00 00 00
00000010  00 00 00 01 00 00 00 00  00 00 00 01 00 00 00 00
00000020  00 00 00 01
}
```

正如我们所看到的，oldExam 函数中的那些 uint64 参数在被调用后首先被转换为 uint256 参数。以上两项的油费分别为 **22235** 和 **21816** ，这意味着新的检查功能节省了 **419** 油费。参数越分散，采用位压缩方法节省的资源就越多。

*   ***配料***

批处理减少了常见的数据处理，从而降低了天然气成本。以下面的代码片段为例:

```
*Old: func once(uint256 header, uint256 val...) x N
New: func batch(uint256 header, uint256[] val... x N)*
```

执行旧函数 N 次处理公共“header”字段 N 次，以及调用函数 N 次。然而，通过批处理，该函数只被调用一次，并且公共的“header”字段只被处理一次。因此，它通过减少“CALLDATALOAD”、内存分配和函数调用操作来节省 gas 成本。N 越大，采用批处理机制节省的资源就越多。DEx.top 的配料气性价比已经公布 [***此处***](https://github.com/dexDev/DEx.top/blob/master/smart-contract/dextop-gas-cost.md) 。

*   ***分离写入存储结构***

在许多情况下，将写入分离到存储中定义的结构对象可以减少气体成本。以下面的代码片段为例:

```
*pragma solidity ^0.4.21;**contract structWrite {
  struct Object {
    uint64 v1;
    uint64 v2;
    uint64 v3;
    uint64 v4;
  }* *Object obj;* *function StructWrite() public {
    obj.v1 = 1;
    obj.v2 = 1;
    obj.v3 = 1;
    obj.v4 = 1;
  }* *function oldExam(uint64 a, uint64 b) public {
    uint a0; uint a1; uint a2; uint a3; uint a4; uint a5; uint a6; 
    uint b0; uint b1; uint b2; uint b3; uint b4; uint b5;* *obj.v1 = a + b;
    obj.v2 = a - b;
    obj.v3 = a * b;
    obj.v4 = a / (b + 1);
  }* *function setObject(uint64 v1, uint64 v2, uint64 v3, uint64 v4) private {
    obj.v1 = v1;
    obj.v2 = v2;
    obj.v3 = v3;
    obj.v4 = v4;
  }* *function newExam(uint64 a, uint64 b) public {
    uint a0; uint a1; uint a2; uint a3; uint a4;
    uint b0; uint b1; uint b2; uint b3;
    setObject(a + b, a - b, a * b, a / (b + 1));
  }
}*
```

对于上面的例子，一旦优化选项被打开，SOLC 以这样的方式编译存储结构写，使得 oldExam 函数将为对象中的每个结构字段招致 1 次 EVM“SSTORE”操作(“SSTORE”是最耗费气体的操作)，因为当没有足够的堆栈空间(只有 16 个局部变量的堆栈空间)时，SOLC 的当前实现不能优化“s store”操作。然而，使用新的方法，它具有充足的堆栈空间，使得当前的编译器在将写合并到结构之后，总共只执行一次“SSTORE”操作，就可以优化结构访问。这可以通过查看相应的汇编代码来轻松验证。这仅限于 SOLC 的当前实施，将来可能会发生变化。对于上述示例，气体成本分别为 **58140** 和 **27318** ，表明新检查功能节省了 **30822** 的气体成本。当堆栈空间不够时，如果利用新的方法，结构的字段越多，获得的气侵就越多。

*   ***uint256 和直接内存访问***

SOLC 的计算单位是 uint256。因此，其他类型(例如，uint8)需要在应用计算之前进行类型转换。这导致了额外的汽油费用。此外，直接内存访问比直接存储访问更具 GC 效率，也比基于结构指针的访问更具 GC 效率。这些小技巧可以表示为:

```
*uint8 data;* ***=>* *** uint256 data;**uint256 val = storageData;   =>   uint256 memoryData = storageData;
(N Times)                         uint256 val = memoryData;**uint64 val = obj.v1;* ***=>* *** uint64 val = val1;*
```

*   ***装配优化***

当编译你的 SOLC 代码时，确保用 SOLC 的“优化-运行”选项进行 GC 实验，以找出在 EVM 运行的最佳 GC 效率汇编代码。

**参考**

*   黄皮书中的天然气成本。 [***链接***](https://docs.google.com/spreadsheets/d/1n6mRqkBz3iWcOlRem_mO09GtSKEKrAsfO7Frgx18pNU/edit#gid=0) 。