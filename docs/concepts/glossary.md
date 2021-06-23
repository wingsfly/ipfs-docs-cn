---
title: 术语
sidebarDepth: 0
description: A glossary guide to key terminology for IPFS, the InterPlanetary File System.
related:
  'Guide to libp2p terminology': https://docs.libp2p.io/reference/glossary/
  'Article: The Decentralized Web Explained in Words You Can Understand (Breaker)': https://breakermag.com/the-decentralized-web-explained-in-words-you-can-understand/
  'Decentralized Web Primer': https://github.com/ipfs-shipyard/ipfs-primer
---

# IPFS术语

[A](#a) [B](#b) [C](#c) [D](#d) [E](#e) [F](#f) [G](#g) [H](#h) [I](#i) [J](#j) [K](#k) [L](#l) [M](#m) [N](#n) [O](#o) [P](#p) [Q](#q) [R](#r) [S](#s) [T](#t) [U](#u) [V](#v) [W](#w) [X](#x) [Y](#y) [Z](#z)

## A

### Announcing - 发布

Announcing 是[libp2p](#libp2p)中的IPFS网络层的一个功能，用于一个节点告知其他节点，它所拥有的可用数据块。

## B

### Bitswap

Bitswap是IPFS最重要的块交换协议。它用于向其他网络中的节点请求块数据或者发送块数据。[更多Bitswap信息](https://github.com/ipfs/specs/blob/master/BITSWAP.md)

### BitTorrent

BitTorrent是一个用于点对点文件共享的通信协议，用于通过互联网分发数据和电子文件。同时也代指第一个使用该协议的文件共享应用。[BitTorrent协议更多信息](https://en.wikipedia.org/wiki/BitTorrent)和[BitTorrent应用](https://www.bittorrent.com)

### Blockchain - 区块链

区块链是一个持续增长的记录列表，其记录称为区块，通过加密算法链接起来形成链表。每个区块包含了前一区块的加密hash、一个时间戳信息、以及交易数据（通常以Merkle树形式表达）。[更多Blockchain信息](https://en.wikipedia.org/wiki/Blockchain)

### Block - 块

Block表示一个二进制的数据块，通过[CID](#cid)来标识。

### Bootstrap Node - 引导节点

引导节点指IPFS网络中的可信节点，IPFS通过它来访问到网络中的其他节点。[更多引导相关的信息Bootstrapping](https://docs.ipfs.io/how-to/modify-bootstrap-list/)

## C

### CBOR

简单的二进制对象表达，Concise Binary Object Representation (CBOR)，是一个基于[JSON](#json)的数据格式，具备较小的代码和消息大小，并具备可扩展性。其在[IPLD](#ipld)中使用。[更多CBOR信息](http://cbor.io/)

### CID

内容标识符(CID)是一个自描述的内容寻址标签，用于指向存储与IPFS中的数据。它是IPFS和[IPLD](#ipld)中最核心的标识符。[更多CID信息](https://docs.ipfs.io/concepts/content-addressing/)

### CID v0

Version 0 (v0)的IPFS内容标识符。46字符长度，以"Qm"开头。使用基本的58编码的多重哈希，非常简单，但相对于新的CID不够灵活。[更多CID v0信息](https://docs.ipfs.io/concepts/content-addressing/#version-0-v0)

### CID v1

Version 1 (v1)的IPFS内容标识符。此版本包含一些前导标识符，提供了向前兼容性，从而可以支持未来不同版本格式的CID。[更多CID v1信息](https://docs.ipfs.io/concepts/content-addressing/#version-1-v1)

### CRDT

无冲突复制数据类型，Conflict-Free Replicated Data Type (CRDT)是一种专门设计的数据结构，用于实现强最终一致性（SEC）和单调性（没有回滚）。[更多CRDT信息](https://github.com/ipfs/research-CRDT)

## D

### Daemon

守护进程是计算机中的一个程序，通常在后台运行。IPFS守护进程使得你的节点对IPFS网络一直在线。[更多IPFS守护进程信息](https://docs.ipfs.io/how-to/command-line-quick-start/#take-your-node-online)

### DAG

有向无环图，Directed Acyclic Graph (DAG)，是计算机科学中的数据结构，适用于版本化的文件系统，区块链，以及针对许多不同类型的信息进行建模。[更多DAG信息](https://en.wikipedia.org/wiki/Directed_acyclic_graph)

### DataStore

数据存储是IPFS节点使用的磁盘文件系统。可以通过配置参数来设置数据存储的位置、大小、构造和操作。[更多Datastore信息](https://github.com/ipfs/go-ipfs/blob/master/docs/config.md#datastore)

### DHT

分布式哈希表，Distributed Hash Table (DHT)是一个分布式的键值存储，其中键为加密哈希。在IPFS中，每个节点都要作为IPFS DHT的一个子集存在。[更多DHT信息](https://docs.ipfs.io/concepts/dht/)

### Dialing

拨号是[libp2p](#libp2p)中IPFS网络层的一个功能，用于打开一个到其他节点的连接。拨号和[监听](#listening)一起构成了[传输](#transport")。

### DNSLink

DNSLink是一个协议，能直接从DNS中链接到内容和服务。一个DNSLink地址看起来像IPNS地址，但使用域名来替代哈希公钥，类似于/ipns/mydomain.org。[更多DNSLink信息](https://dnslink.io/)

### DWeb

去中心化网络(DWeb)看起来很像当前的万维网，但构建于新的支持去中心化方案的底层技术。任何一个单一实体（如政府或者恐怖组织）都很难删除任何一个网页、网站或者服务，不论是故意的还是偶然的。

## E

## F

### Filestore

文件存储是一个数据存储，它在文件系统中将块的[UnixFS](#unixfs)数据组件存为文件而非块数据。这允许将内容添加到IPFS中，而无需在IPFS数据存储中复制内容。

## G

### Gateway

IPFS网关是IPFS和传统网页浏览器之间的桥梁。通过网关用户可以访问IPFS上的文件和网站，就好像它们存放于传统的网站服务器中一样。[更多Gateway信息](https://github.com/ipfs/go-ipfs/blob/master/docs/gateway.md)

### 垃圾回收

垃圾回收(GC)是每个IPFS节点中的文件及块缓存的清除操作。节点需要将之前缓存的资源清除，从而为新的资源让出空间。[固定资源](#pinning)从来不会被删除。

### Graph

计算机科学中，图是一个来源于数学中图论领域的抽象数据类型。IPFS中的[Merkle-DAG](#merkledag)是一个特定的图结构。

### Graphsync

Graphsync是一个讨论中的内容复制协议替代方案，类似于[Bitswap](#bitswap)。和Bitswap一样，其主要工作是在节点间同步数据块。[更多Graphsync内容](https://github.com/ipld/specs/blob/master/block-layer/graphsync/graphsync.md)

## H

### Hash

A Cryptographic Hash is a function that takes some arbitrary input (content) and returns a fixed-length value. The exact same input data will always generate the same hash as output. There are numerous hash algorithms. [More about Hash](https://docs.ipfs.io/concepts/hashing/)

## I

### Information Space

Information Space is the set of concepts, and relations among them, held by an information system. This can be thought of as a conceptual framework or tool for studying how knowledge and information are codified, abstracted, and diffused through a social system. [More about Information Space](https://en.wikipedia.org/wiki/Information_space)

### IPLD

The InterPlanetary Linked Data (IPLD) model is a set of specifications in support of decentralized data structures for the content-addressable web. Key features are interoperable protocols, easily upgradeable, backward compatible. A single namespace for all hash-based protocols. [More about IPLD](https://ipld.io/)

### IPNS

The InterPlanetary Name System (IPNS) is a system for creating and updating mutable links to IPFS content. IPNS allows for publishing the latest version of any IPFS content, even though the underlying IPFS hash has changed. [More about IPNS](https://docs.ipfs.io/concepts/ipns/)

## J

### JSON

JavaScript Object Notation (JSON) is a lightweight data-interchange format. JSON is a text format that is completely language independent, human-readable, and easy to parse and generate. [More about JSON](https://www.json.org/)

## K

## L

### libp2p

The libp2p project is a modular system of protocols, specifications, and libraries that enable the development of peer-to-peer network applications. It is an essential component of IPFS. [More about libp2p](https://libp2p.io/)

### Listening

Listening is a function of the IPFS networking layer in libp2p, wherein an incoming connection is accepted from another peer. Together, an implementation of [dialing](#dialing) and listening forms a [transport](#transport).

## M

### Merkle-DAG

The Merkle-DAG is a computer science data structure used at the core of IPFS files/block storage. Merkle-DAGs create a hash to their content, known as a [Content Identifier](#cid). [More about Merkle-DAG](https://docs.ipfs.io/concepts/merkle-dag/)

### Merkle Forest

Merkle Forest is a phrase coined to describe the distributed, authenticated, hash-linked data structures (Merkle trees) running technologies like Bitcoin, Ethereum, git, and BitTorrent. In this way, IPFS is a forest of linked Merkle trees. [More about Merkle Forest](https://www.youtube.com/watch?v=Bqs_LzBjQyk)

### Merkle Tree

A Merkle Tree is a specific type of hash tree used in cryptography and computer science, allowing efficient and secure verification of the contents of large data structures. Named after Ralph Merkle, who patented it in 1979. [More about Merkle Tree](https://en.wikipedia.org/wiki/Merkle_tree)

### MFS

The Mutable File System (MFS) is a tool built into IPFS that lets you treat files like a normal name-based filesystem. You may add, edit, and remove MFS files while all link updates and hashes are taken care of for you. [More about MFS](https://docs.ipfs.io/concepts/file-systems/#mutable-file-system-mfs)

### Multibase

Multibase is a protocol for disambiguating the encoding of base-encoded (e.g. base32, base36, base64, base58, etc.) binary appearing in text. In IPFS, it is used as a prefix specifying the encoding used for the remainder of the CID. [More about Multibase](https://github.com/multiformats/multibase#readme)

### Multicodec

Multicodec is an identifier indicating the format of the target content. It helps people and software know how to interpret that content after the content is fetched. In IPFS, it is backed by an agreed-upon codec table. It is designed for use in binary representations, such as keys or identifiers (i.e [CIDv1](#cid)). [More about Multicodec](https://github.com/multiformats/multicodec#readme)

### Multihash

Multihash is a protocol for differentiating outputs from various well-established hash functions, addressing size and encoding considerations. It is useful to write applications that future-proof their use of hashes, and it allows multiple hash functions to coexist. [More about Multihash](https://multiformats.io/multihash/).

### Multiformats

The Multiformats project is a collection of protocols that aim to future-proof systems today. A key element is enhancing format values with self-description. This allows for interoperability, protocol agility, and promotes extensibility. [More about Multiformats](https://multiformats.io/) and [Multihash](https://multiformats.io/multihash/)

## N

### Node

A Node or [peer](#peer) is the IPFS program that you run on your local computer to store/cache files and then connect to the IPFS network (by running the [daemon](#daemon)). [More about Node](https://docs.ipfs.io/how-to/command-line-quick-start/#take-your-node-online)

## O

## P

### Path/Address

A Path/Address is the method within IPFS of referencing content on the web. Addresses for content are path-like; they are components separated by slashes. [More about Path/Address](https://docs.ipfs.io/how-to/address-ipfs-on-web/)

### Peer

In system architecture, a Peer is an equal player in the peer-to-peer model of decentralization, as opposed to the client-server model of centralization. [See also Peer as Node](#node)

### Peer ID

A Peer ID is how each unique IPFS node is identified on the network. The Peer ID is created when the IPFS node is initialized and is essentially a cryptographic hash of the node's public key. [More about Peer ID](https://docs.ipfs.io/concepts/dht/#peer-ids)

### Pinning

Pinning is the method of telling an IPFS node that particular data is important and so it will never be removed from that node's cache. To learn more, start by understanding [persistence, permanence, and pinning](/concepts/persistence/); then, see how to [add local pin](/how-to/pin-files/) and read [what remote pins are](#remote-pinning).

### Pinning Service API

A vendor-agnostic [API specification](https://ipfs.github.io/pinning-services-api-spec/) that anyone can implement to provide a service for [remote pinning](#remote-pinning).

### Pubsub

Publish-subscribe (Pubsub) is an experimental feature in IPFS. Publishers send messages classified by topic or content, and subscribers receive only the messages they are interested in. [More about Pubsub](https://blog.ipfs.io/25-pubsub/)

## Q

## R

### Remote Pinning - 远程固定

[固定](#pinning)的一个变种，使用第三方服务来确保数据在IPFS中的持久性，即便你的本地节点已经下线或者本地数据副本已经在垃圾回收中清除了。[更多与远程固定服务协作的信息](/how-to/work-with-pinning-services/)

### Relay - 中继

中继是在彼此无法直接建立连接的libp2p节点（如IPFS节点）间建立连接的一种方式。无法直接连接可能是因为节点位于NAT、反向代理、防火墙等的后面。[更多Relay信息](https://github.com/libp2p/specs/tree/master/relay)

### Repo - 仓库

仓库(Repo)是IPFS存储其设置和内部数据的目录。它由`ipfs init`命令创建。[更多Repo信息](https://docs.ipfs.io/how-to/command-line-quick-start/#install-ipfs)

## S

### SFS

自认证文件系统(SFS)是一个分布式文件系统，不需要特殊的数据交换权限。它是自我认证的，因为提供给客户端的数据通过文件名（由服务端进行签名）就完成了身份验证。[更多SFS信息](https://en.wikipedia.org/wiki/Self-certifying_File_System)

### Signing (Cryptographic)

加密方式的数据签名可以允许信任来自不受信任来源的数据。加密数据签名的值可以通过不受信任的通道进行传输，对数据的任何篡改都会被监测到。[更多数字签名的信息](https://en.wikipedia.org/wiki/Digital_signature)

### Swarm

Swarm是一个代指IPFS节点网络的术语，你的节点与之相连接。Swarm地址就是你的本地节点用于监听其他IPFS节点连接的地址。[更多Swarm地址信息](https://docs.ipfs.io/how-to/configure-node/#addresses)

## T

### Transport

在[libp2p](#libp2p)中，传输协议指的是将数据从一台机器转移到另一台的技术。这可以是TCP传输，浏览器中的WebSocket连接，或者任何能够实现传输接口的方案。

## U

### UnixFS

Unix文件系统(UnixFS)是IPFS中用于表示文件及其包含的所有链接和元数据的数据格式。它大致是基于Unix的文件管理机制。向IPFS中添加一个文件会以UnixFS格式创建一个块，或者一个块树，并使它不会被垃圾回收。[更多UnixFS信息](https://docs.ipfs.io/concepts/file-systems/#unix-file-system-unixfs)

## V

## W

## X

## Y

## Z
