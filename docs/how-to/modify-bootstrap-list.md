---
title: 修改引导列表
legacyUrl: https://docs.ipfs.io/guides/examples/bootstrap/
description: Learn how to modify the IPFS bootstrap peers list to create a personal IPFS network.
---

# 修改引导节点列表

IPFS引导列表是一个对等节点的列表，IPFS守护进程可以从这里了解到网络中其他节点的信息。IPFS带有一个默认的信任节点列表，你也可以按需自行修改这个列表。通常可以自定义一个引导列表来创建一个私有的IPFS网络。

首先，列出当前节点的引导列表：

```bash
ipfs bootstrap list
> /dnsaddr/bootstrap.libp2p.io/p2p/QmNnooDu7bfjPFoTZYxMNLWUQJyrVwtbZg5gBMjTezGAJN
> /dnsaddr/bootstrap.libp2p.io/p2p/QmQCU2EcMqAqQPR2i9bChDtGNJchTbq5TbXJJ16u19uLTa
> /dnsaddr/bootstrap.libp2p.io/p2p/QmbLHAnMoJPWSCR5Zhtx6BHJX9KiKNN6tpvbUcqanj75Nb
> /dnsaddr/bootstrap.libp2p.io/p2p/QmcZf59bWwK5XFi76CZX8cbJ4BhTzzA3gU1ZjYZcYW3dwt
> /ip4/104.131.131.82/tcp/4001/p2p/QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ
```

以上列出的是默认的IPFS引导节点的地址，它们是由IPFS开发团队运行维护。列出的地址完全按照[multiaddr](https://github.com/multiformats/multiaddr)格式规范定义和解析，使得每个协议都很明确。这样你的节点就确切知道如何访问到这些引导节点 - 这些地址都是很明确的。

除非你知道你行为的具体影响，不要修改以上的列表。引导是分布式系统中一个重要的安全故障点：恶意的引导节点只会把你介绍到其他的恶意节点处。建议一直保留IPFS开发团队所提供的默认列表，或者在配置私有网络的情形下，才指定一个你自己控制的节点列表。不要把不信任的节点加入到这个列表中来。

这里我们将添加一个新的节点到引导列表中：

```bash
> ipfs bootstrap add /ip4/25.196.147.100/tcp/4001/p2p/QmaMqSwWShsPg2RbredZtoneFjXhim7AQkqbLxib45Lx4S
```

然后我们从引导列表中移除一个节点：

```bash
> ipfs bootstrap rm /ip4/104.131.131.82/tcp/4001/p2p/QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ
```

此时我们可以创建一个新引导列表的备份。可以通过重定向`ipfs bootstrap list`的输出到一个文件中来简单实现：

```bash
> ipfs bootstrap list >save
```

如果我们想从头开始，可以立即删除整个引导列表：

```bash
> ipfs bootstrap rm --all
```

在空列表的情形下，我们可以恢复默认的引导列表：

```bash
> ipfs bootstrap add --default
```

再次移除整个引导列表，然后通过管道连接将保存文件的内容送给`ipfs bootstrap add`，从而恢复之前保存的那份列表：

```bash
> ipfs bootstrap rm --all
> cat save | ipfs bootstrap add
```
