---
title: IPFS如何运作
legacyUrl: https://docs.ipfs.io/introduction/how-ipfs-works/
description: Learn how the InterPlanetary File System (IPFS) works and why it's an essential part of the future internet.
---

# IPFS如何运作

::: tip
可以查看来自IPFS Camp 2019的内容[Core Course: How IPFS Deals With Files](https://www.youtube.com/watch?v=Z5zNPwMDYGg)，作为IPFS通常如何处理文件的视频概述。
:::

IPFS是一个点对点(p2p)的存储网络。内容可以通过全球任意位置的节点来访问到，这些节点可以传递信息或者存储信息，也可能同时进行。IPFS知道如何通过内容地址而非它的位置来找到请求内容。

可以从三个基本原则来理解IPFS：

1. 基于内容寻址的唯一标识符
2. 通过有向无环图（DAG）的内容链接
3. 通过分布式哈希表（DHT）的内容发现

这三个原则在彼此之上互相组织，形成了IPFS的生态系统。我们从内容寻址和内容的唯一标识符开始了解。

## 内容寻址

IPFS使用内容地址来标识具体的内容，而非使用它的位置。我们平时一直都在通过内容来查询事物。例如，我们在图书馆中找一本书时，我们通常通过书名来查找，这就是内容寻址，因为我们在查询它是什么。如果我们是通过位置寻址来找这本书的话，我们会查询它在哪里：“我需要在二楼，第一排从下往上数第三层架子上，从左数的第四本书”。如果有人移动了这本书，那你就很不走运了。

这也是当前互联网和电脑系统的问题！当前内容也都是基于地址定位的，如：

- `https://en.wikipedia.org/wiki/Aardvark`
- `/Users/Alice/Documents/term_paper.doc`
- `C:\Users\Joe\My Documents\project_sprint_presentation.ppt`

相比之下，IPFS协议中的每片内容都有一个[内容标识符](/concepts/content-addressing/)，或称为CID，对应于它的hash值。这个hash值对每个内容来说都是唯一的，虽然它看起来要比原始内容短很多。如果你不熟悉hash，在这里查看说明：[加密哈希指南](/concepts/hashing/)。

许多分布式系统都使用内容寻址的哈希值，不仅仅作为内容标识，还用于将其链接起来，从提交代码到区块链中加密货币的运行都是用了这个策略。然而，这些系统之间的底层数据结构不一定是可互相操作的。

这正是[星际关联数据 - Interplanetary Linked Data (IPLD)项目](https://ipld.io/)的用武之处。IPLD在散列链接的文件结构间进行转换，以在不同的分布式系统中达成数据的统一。IPLD提供了用于组合式热插拔的模块库（解析每种可能的IPLD节点解析器），来解析路径、选择器或者跨多个链接节点的查询请求，以允许忽略底层协议来浏览数据。IPLD提供了一种方式以转换不同的基于内容寻址的数据结构，从而使得：“你用的是Git样式的数据，没关系，我可以关注这些链接。你用的是以太坊，没问题，我也可以关注这些链接！”。

IPFS遵循特定的数据结构偏好和约定。IPFS协议使用IPLD及这些约定，从IPFS网络中通过唯一标识内容的IPFS地址来获取原始内容。下面将探讨内容地址是如何通过DAG数据结构，将内容之间的链接嵌入进来的。

## 有向无环图(DAG)

IPFS和许多分布式系统都利用了一个数据结构，名为[有向无环图](https://en.wikipedia.org/wiki/Directed_acyclic_graph)或者DAG。具体来说，用的是 _Merkle DAGs_，这种DAG的每个节点都有一个唯一标识，对应的是这个节点内容的hash。听起来很熟悉吧，这个对应于我们前面说到的CID概念。换句话说，内容寻址就是通过其hash值来标识一个数据对象（如Merkle DAG节点）。阅读[Merkle DAG指南](/concepts/merkle-dag/)，以更深入的了解该主题。

IPFS使用了针对表示目录和文件优化过的Merkle DAG，你也可以用很多不同方式来构建Merkle DAG。如Git使用的Merkle DAG中包含了许多版本的仓库信息。

IPFS构建一个表达内容的Merkle DAG时，通常会先把内容切分为块（block）。内容分成小块意味着文件的不同部分可以来自于不同的源，而且可以更快的被校验。（如果使用过BitTorrent，就会注意到下载文件的时候，BitTorrent可以一次从多个节点获取文件，这里也是一样的方式）

::: tip

使用[DAG Builder visualizer](https://dag.ipfs.io/)可以很容易的查看一个文件的Merkle DAG描述。
:::


Merkle DAG有点像["海龟一路向下"](https://ipfs.io/ipfs/QmXoypizjW3WknFiJnKLwHCnL72vedxjQkDDP1mXWo6uco/wiki/Turtles_all_the_way_down.html)的场景；即，任何事物都有一个CID。假如你有一个文件，通过它的CID标识了此文件。如果这个文件在一个文件夹中，其中还有其他的文件，这些文件也都有CID，那么这个文件夹的CID是什么样的呢？它会是这个文件夹下所有文件（也就是这个文件夹的内容）CID的hash值。同样，这些文件是由块组成的，每个块也都有一个CID。你可以查看你电脑的文件系统是怎么样表示为DAG的，还可以看到Merkle DAG是怎样形成的。可以查看[IPLD Explorer](https://explore.ipld.io/#/explore/QmSnuWmxptJZdLJpKRarxBMS2Ju2oANVrgbr2xWbie9b2D)，以对这个概念进行可视化探索。

Merkle DAG以及将内容切分为块的另一个有用特性在于：如果有两个相似的文件，他们可以共享一部分的Merkle DAG，即不同的Merkle DAG可以引用相同的子集。例如你正在更新一个网站，只有更新后的文件产生了新的内容地址，因此老版本和新版本可以引用所有其他的原有块。这可以让超大数据集（如基因数据或者气象数据）的版本更新更加高效，因为每次只需要传输哪些新产生的或者被修改的部分，而不用传输整个文件。

所以回顾一下，IPFS为内容生成了CID，然后将它们连接在一起组成了Merkle DAG。现在我们可以进入最后一个部分：如何发现和迁移内容。

## 分布式哈希表(DHTs)

为了找到哪个节点托管了你正在寻找的内容（内容发现），IPFS使用了[分布式哈希表](/concepts/dht/)，即DHT。哈希表是一个键值数据库，分布式的哈希表将表拆分到分布式网络的所有节点中。为了找到内容，可以查询这些节点。


[libp2p项目](https://libp2p.io/)项目是IPFS生态系统的一部分，提供了DHT功能，处理节点间的连接，以及相互之间的通讯服务。（注意，和IPLD一起，libp2p也可以作为其他分布式系统的工具，儿不仅仅是为IPFS服务。）

一旦你知道了内容所处的位置（或者更准确的说，知道哪些节点分别存储了你寻找的文件的每一块数据），你可以再次通过DHT找到这些节点的当前位置（内容路由）。因此为了获取到内容，你需要使用libp2p查询两次。

现在已经发现了需要内容，并且找到了它当前所在的位置。然后就需要连接到内容所在节点以获取它（内容交换）。IPFS当前使用了一个模块[_Bitswap_](https://github.com/ipfs/specs/blob/master/BITSWAP.md)，用于请求和发送块数据。Bitswap允许你连接到拥有你所需要数据的一组节点上，发送你的请求列表（一个块列表，包含所有你想要的块信息），然后请求他们发送你需要的块数据。一旦收到这些块，你可以通过计算这些内容的hash，得到对应的CID并和你请求的CID进行对比，以验证数据的有效性。这些CID也可以在需要时用于删除重复数据块。

也还有[讨论中的其他内容复制协议](https://github.com/ipfs/camp/blob/master/DEEP_DIVES/24-replication-protocol.md)，其中开发进展最好的是[_Graphsync_](https://github.com/ipld/specs/blob/master/block-layer/graphsync/graphsync.md)。还有一个正在讨论的提议，[扩展Bitswap协议](https://github.com/ipfs/go-bitswap/issues/186)以添加请求/响应相关的功能。

### Libp2p

libp2p中非常有用的特性之一就是连接复用。传统情形下，每个系统服务都会打开不同的连接来和远程的其他同类服务进行通信。在IPFS中，你只需要打开一个连接，每个服务都会去复用它。你节点需要向其他节点交互任何信息时，额外发送信息对应的一些描述，另一端就会知道如何将这些信息块进行分类，以决定它的归属服务。

这非常有用，因为连接通常很难建立，同时维护成本也很高。采用多路复用的话，一旦你建立了连接，你就可以做任何期望通过它来完成的事情了。

## 模块化范式

正如你可能从本章节注意到的，IPFS生态是由多个模块化的库组成的，这些库可以支持任何分布式系统的相应部分的功能。你可以以任何新的方式来单独使用或者组合使用这些模块的任意部分。

IPFS生态系统为内容生成了CID并通过IPLD Merkle DAG来把内容链接聚合起来。你可以使用libp2p的DHT功能来发现内容，连接到该内容的任意一个提供者那里，然后通过多路复用的链接来下载它。所有这些都是由其最核心的特性：可链接的，唯一的标识(CID)来聚合在一起；这些构成了IPFS的最基础的部分。
