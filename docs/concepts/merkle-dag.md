---
title: Merkle DAGs
legacyUrl: https://docs.ipfs.io/guides/concepts/merkle-dag/
description: Learn about Merkle Directed Acyclic Graphs (DAGs) and why they're important to IPFS.
---

# Merkle有向无环图(DAG)

::: callout
可以在ProtoSchool的教程中深入了解这个强大的，基于内容寻址的数据结构，[Merkle DAGs: Structuring Data for the Distributed Web](https://proto.school/merkle-dags).
:::

Merkle DAG是一个有向无环图，它的每个节点都有一个标识符，值为节点内容的哈希值（内容包括节点携带的不透明负载，以及其子节点的标识符列表），使用的是如SHA256的加密hash算法。这带来了一些重要的考虑因素：

- Merkle DAGs只能从叶子节点-即没有子节点的节点开始构建。父节点在子节点之后添加，因为需要提前计算子节点的标识符以链接它们。
- Merkle DAG中的每个节点也是它自身的（子）Merkle DAG的根，其子图包含在父DAG中。
- Merkle DAG节点是不可变的。节点的任何修改都会改变它的标识符，从而影响DAG的所有上一级，基本上创建了一个新的DAG。查看来自Consensys公司一个朋友的[this helpful illustration using bananas](https://media.consensys.net/ever-wonder-how-merkle-trees-work-c2f8b7100ed3)。

Merkle DAG和Merkle树很相似，但是没有平衡化要求，且每个节点都可以携带一个负载。在DAG中，多个分叉可以重新收敛，换而言之一个节点可以有多个父节点。

通过数据对象（如同Merkle DAG的节点）的hash值来标识它称为内容寻址。因此我们把节点标识符称为[_内容标识符_](/concepts/content-addressing/)，或CID。

以之前的链表为例，假设每个节点的负载仅仅是它后代的CID，即：_A=Hash(B)→B=Hash(C)→C=Hash(∅)_。hash函数的特性确保了在创建Merkle DAG时不会出现循环/回环。（注意：hash函数是一个单向函数，除非发现并利用了某些弱点，创建循环是近乎不可能的）

Merkle DAG是自我验证的结构。节点的CID明确链接到它的载荷内容和它的后代节点。因此两个具备相同CID的节点，明确表示完全相同的DAG。这将是无需复制完整的DAG即可同步Merkle-CRDTs（无冲突复制数据类型）的关键属性，其为IPFS等系统所利用。Merkle DAG用途非常广泛，代码控制系统如git等使用它来高效的存储仓库历史记录，使得可以在代码分支间检查冲突，并进行重复数据删除。

想了解现实世界中Merkle DAG的应用实例的话，可以使用[DAG可视化构建器](https://dag.ipfs.io/)来查看你选择的文件的Merkle DAG表示。

## 更多资源

- 完整的[Merkle-CRDTs论文草稿](https://hector.link/presentations/merkle-crdts/merkle-crdts.pdf) by [@hsanjuan](https://www.github.com/hsanjuan), [@haadcode](https://www.github.com/haadcode), and [@pgte](https://www.github.com/pgte)
- [DAG可视化构建器](https://dag.ipfs.io/)
