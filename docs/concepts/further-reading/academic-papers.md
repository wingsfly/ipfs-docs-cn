---
title: 学术论文
description: Here are a few papers that are useful for understanding IPFS, whether it be understanding the IPFS specification itself or the background for the decentralized web, protocols, hashing, and so on.
---

# 学术论文

这里是一些有助于理解IPFS的论文，无论是用于理解IPFS规范本身，或者其去中心化网络、协议、哈希等背景信息。

## [IPFS - 基于内容寻址的、版本化的P2P文件系统](https://github.com/ipfs/papers/raw/master/ipfs-cap2pfs/ipfs-p2p-file-system.pdf)

**Benet, Juan**: _星际文件系统(IPFS)是一个点对点的分布式文件系统，旨在将所有计算设备通过相同的文件系统连接起来。在某些方面，IPFS与Web很相似，但IPFS也可以被看作一个BitTorrent集群，在一个Git仓库中交换对象。换而言之，IPFS提供了一个高吞吐的、基于内容寻址的块存储模型，以及内容寻址的超链接。这构成了一个通用的Merkle DAG，一种可以在其之上构建版本化文件系统、区块链甚至是永久性网络的数据结构。IPFS包括一个分布式哈希表，一个激励性的区块交换和一个自我认证的名字空间。IPFS不存在单点故障，节点之间也无需彼此信任。_

## [一个基于密钥的安全路由方案的可行方案](https://web.archive.org/web/20170809130252id_/http://www.tm.uka.de/doc/SKademlia_2007.pdf)

**Baumgart, Ingmar and Mies, Sebastian**: _安全性是完全去中心化的点对点系统的常见问题。尽管已经存在一些关于如何构建一个安全的基于密钥的路由协议的建议，但仍然没有一个可行的方案。在此论文中，我们介绍了一种在Kademlia基础上的基于密钥的安全路由协议，它通过在多个不相交路径上的并行查询，加密难题限定自由节点的生成，使其对常见攻击具备高弹性。同时还引入了可靠的兄弟广播，这可用于以安全，可复制的方式存储数据。我们对提议的Kademlia协议扩展的安全性进行了分析性的评估，并模拟了在对抗性节点影响下，多重不相交路径的查询成功率效果。_

## [使用Coral来民主化发布内容](https://web.archive.org/web/20181117012712/http://www.coralcdn.org/docs/coral-nsdi04.pdf)

**Freedman, Michael J., Freudenthal, Eric and Mazières, David**: _CoralCDN是一个点对点的内容分发网络，其允许用户运行一个网站，并提供高性能、大规模请求和低廉的互联网带宽连接成本保障。运行CoralCDN的志愿者站点在用户访问内容时会自动复制它。通过CoralCDN发布内容和在某个URL上对主机名进行小修改一样简单；有一个点对点的DNS层，能够透明的将浏览器重定向到特定的缓存节点上，这些节点相互协作，以尽量减少原始web服务器的负载。该系统的关键目标之一就在于避免创建热点，这个阻碍志愿者节点并影响性能。它通过Coral来实现这一点，这是一种延迟优化的分层索引架构，构建于一个新的被称为分布式稀疏哈希表或DSHT的抽象架构之上。_

## [通过自我认证的路径名称，摆脱中心化控制的弊端](http://www.sigops.org/ew-history/1998/papers/mazieres.ps)

**Mazières, David and Kaashoek, M. Frans**: _长期以来，人们都是信任中央机构来确保局域网中的安全协作。然而互联网中并没有一个独立组织能提供类似的可管理结构。因此如果全球分布式系统也是依赖于中央机构来保障安全，其用户会面临严重的后果。幸运的是，并不一定要通过中央控制来获取安全性。为证明这一点，我们提出了SFS，一个安全的、全球化的、去中心化的文件系统，允许轻松的跨管理域协作。通过一个简单的想法，自我认证的路径名称，SFS让用户摆脱了中央化控制的弊端。_

## [Kademlia: 一个基于XOR指标的点对点的信息系统](http://pdos.csail.mit.edu/~petar/papers/maymounkov-kademlia-lncs.pdf)

**Mazières, David and Maymounkov, Petar**: _我们描述了一个点对点的分布式哈希表，它在易错的环境下具备可证明的一致性和性能表现。我们的系统使用一种全新的基于XOR的度量拓扑来进行路由查询和节点定位，该拓扑简化了算法并促进了证明。该拓扑具有这样的属性：每次消息交换都会传达或者强化了有用的联系信息。该系统利用此信息发送并行的，异步的查询消息，且可以容忍节点故障，而不会给用户带来超时延迟。_
