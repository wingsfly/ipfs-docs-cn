---
title: Libp2p
sidebarDepth: 0
description: Learn about the Libp2p protocol and why it's an important ingredient in how IPFS works.
related:
  'What is Libp2p?': https://docs.libp2p.io/introduction/what-is-libp2p/
  'Foundational Libp2p concepts': https://docs.libp2p.io/concepts/
  'Getting started with Libp2p': https://docs.libp2p.io/tutorials/getting-started/
  'Examples of Libp2p key features': https://docs.libp2p.io/examples/
---

# Libp2p

[Libp2p](https://libp2p.io/)是一个由协议、规范和库组成的模块化系统，用于实现点对点网络的应用开发。Libp2p作为IPFS项目的一部分被发起，目前也仍然是IPFS的重要组成部分。作为IPFS的网络层，Libp2p针对关键的点对点因素，如传输、安全、节点路由和内容发现都提供了灵活的解决方案。Libp2p已经有了[Go](https://github.com/libp2p/go-libp2p), [JavaScript](https://github.com/libp2p/js-libp2p), [Rust](https://github.com/libp2p/rust-libp2p), [Python](https://github.com/libp2p/py-libp2p), 以及[C++](https://github.com/soramitsu/libp2p)这些语言的实现。

::: callout
libp2p的强大之处在于它的模块化设计。在ProtoSchool的[Introduction to libp2p](https://proto.school/introduction-to-libp2p)教程中了解更多。
:::

## 点对点网络应用

[点对点网络](https://docs.libp2p.io/reference/glossary/#peer-to-peer-p2p)指的是其中的参与者，通常称为对等节点（_"peers"_），彼此之间是作为平等角色直接互相通信。这与传统的[客户端-服务器模型](https://docs.libp2p.io/reference/glossary/#client-server)形成鲜明对比，传统模型下一个高优先的中央服务器会面向网络中的许多客户端程序提供服务，这些客户端之间通常不会彼此通信，而仅仅与中央服务器通信。

使用Libp2p作为其点对点应用的网络层的人们，能够立刻将精力释放出来以专注于应用特定的任务中，因为Libp2p已经处理了大量[去中心化系统的任务](https://hub.packtpub.com/libp2p-the-modular-p2p-network-stack-by-ipfs-for-better-decentralized-computing/)。同时他们还能够自定义Libp2p的关键元素，如传输、身份标识和安全保障。正在使用Libp2p的应用包括[Filecoin](https://filecoin.io/), [Parity](https://www.parity.io/why-libp2p/), 以及[OpenBazaar](https://www.openbazaar.org/)。

### Libp2p特性

#### [寻址](https://docs.libp2p.io/concepts/addressing/)

Libp2p以一致的方式使用了许多不同的寻址方案。多重地址(简写为[multiaddr](https://github.com/multiformats/multiaddr))将多层的地址信息编码为一个单独的“面向未来前瞻性”的路径结构中。如`/ipv4/171.113.242.172/udp/162`，说明它使用的是IPv4协议的地址171.113.242.172，同时它是在162端口发送UDP数据包。

#### [传输](https://docs.libp2p.io/concepts/transport/)

用于将数据从一台机器移到另一台机器的技术。传输有两个核心操作：监听和拨号。监听使你可以接受来自其他节点的入站连接；拨号指的是打开一个到监听节点的出站连接的过程。Libp2p的核心需求之一就是传输不可知，即使用何种传输协议是由应用开发者决定的（他可以同时支持多种不同的传输协议）。

#### [安全](https://docs.libp2p.io/introduction/what-is-libp2p/#security)

Libp2p支持将一个传输连接升级为安全的加密通道。然后你可以信任正在通信的对等节点身份，第三方将无法监听该会话或者修改其中的内容。当前对于IPFS 0.7版本默认使用的是[TLS 1.3](https://www.ietf.org/blog/tls13/)。之前的默认项[SECIO](https://docs.libp2p.io/concepts/secure-comms/)现在已经弃用，且默认被禁用。(查看[这篇博客](https://blog.ipfs.io/2020-08-07-deprecating-secio/)以了解更多信息)

#### [节点标识](https://docs.libp2p.io/concepts/peer-id/)

节点标识([通常称为节点ID](https://docs.libp2p.io/reference/glossary/#peerid))是对在点对点网络中一个特定节点的唯一引用。每个Libp2p节点都有一个私钥，其对所有其他节点保密；以及一个对应的公钥，用于分享给其他节点。节点ID就是节点公钥的[加密hash](https://en.wikipedia.org/wiki/Cryptographic_hash_function)。节点ID采用的是[multihash](https://docs.libp2p.io/reference/glossary/#multihash)编码格式。

#### [节点路由](https://docs.libp2p.io/introduction/what-is-libp2p/#peer-routing)

节点路由指通过其他节点的已有信息，发现节点对应的地址的过程。在一个节点路由系统中，一个节点要么有相关信息，直接把我们要的节点地址提供给我们；要么将我们的查询发送到另一个更可能知道答案的节点。Libp2p中的节点路由采用[分布式哈希表](https://docs.libp2p.io/reference/glossary/#dht)，通过[Kademlia](https://en.wikipedia.org/wiki/Kademlia)路由算法来迭代将请求发送到逐步接近所需节点ID的位置。

#### [内容发现](https://docs.libp2p.io/introduction/what-is-libp2p/#content-discovery)

在内容发现中，可以请求特定的数据，但无需考虑是由谁提供的，因为你可以验证数据的完整性。Libp2p为此提供了一个[内容路由接口](https://github.com/libp2p/interface-content-routing)，其采用了与节点路由一致的[Kademlia](https://en.wikipedia.org/wiki/Kademlia)算法来可靠实现。

#### [NAT穿透](https://docs.libp2p.io/concepts/nat/)

网络地址转换(NAT)允许你在网络边界处无缝的转换流量。NAT能够将一个地址映射为另一个地址。通常NAT可以透明的处理传出连接，但对传入连接的监听需要做一些配置。Libp2p已经实现了以下的NAT穿透方式：[自动路由配置](https://docs.libp2p.io/concepts/nat/#automatic-router-configuration), [打洞(STUN)](https://docs.libp2p.io/concepts/nat/#hole-punching-stun), [AutoNAT](https://docs.libp2p.io/concepts/nat/#autonat), 以及[回环中继(TURN)](https://docs.libp2p.io/concepts/nat/#circuit-relay-turn)。

#### [协议](https://docs.libp2p.io/concepts/protocols/)

以下是Libp2p自带的协议，使用了Libp2p的核心抽象概念如[传输](https://docs.libp2p.io/concepts/transport/), [节点标识](https://docs.libp2p.io/concepts/peer-id/), 和[寻址](https://docs.libp2p.io/concepts/addressing/)。每个Libp2p协议都有一个唯一的字符串标识符，用于在首次建立连接时的[协议协商](https://docs.libp2p.io/concepts/protocols/#protocol-negotiation)过程。Libp2p核心协议包括[Ping](https://docs.libp2p.io/concepts/protocols/#ping), [Identify](https://docs.libp2p.io/concepts/protocols/#identify), [secio](https://docs.libp2p.io/concepts/protocols/#secio), [kad-dht](https://docs.libp2p.io/concepts/protocols/#kad-dht), 和[Circuit Relay](https://docs.libp2p.io/concepts/protocols/#circuit-relay).

#### [数据流多路复用](https://docs.libp2p.io/concepts/stream-multiplexing/)

通常简称为流复用，它允许多个独立的逻辑流共享一个共同的底层传输介质。Libp2p的流复用位于传输层之上，因此允许不同的流使用同一个TCP端口或者其他的原始传输连接。当前的多路复用实现包括[mplex](https://docs.libp2p.io/concepts/stream-multiplexing/#mplex), [yamux](https://docs.libp2p.io/concepts/stream-multiplexing/#yamux), [quic](https://docs.libp2p.io/concepts/stream-multiplexing/#quic), [spdy](https://docs.libp2p.io/concepts/stream-multiplexing/#spdy), 以及[muxado](https://docs.libp2p.io/concepts/stream-multiplexing/#muxado)。

#### [发布和订阅](https://docs.libp2p.io/concepts/publish-subscribe/)

通常简称为 _pub-sub_，这是一个节点间基于感兴趣的主题而聚集在一起的系统。节点对一个主题感兴趣称为订阅该主题，节点也可以发送消息到某个主题中，它会传输到所有订阅了该主题的节点上。pub-sub的示例应用包括聊天室和文件共享。关于更多其他的pub-sub设计讨论，可以查看[gossipsub specification](https://github.com/libp2p/specs/blob/master/pubsub/gossipsub/README.md)。

## 更多资源

- [What is Libp2p?](https://docs.libp2p.io/introduction/what-is-libp2p/)
- [Introduction to Libp2p](https://www.youtube.com/embed/CRe_oDtfRLw)
- [Getting started with Libp2p](https://docs.libp2p.io/tutorials/getting-started/)
- [The Libp2p glossary](https://docs.libp2p.io/reference/glossary/)
- [The Libp2p specification](https://github.com/libp2p/specs)
