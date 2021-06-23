---
title: Bitswap
sidebarDepth: 0
description: Learn about Bitswap and how it plays into the overall architecture of IPFS, the InterPlanetary File System.
related:
  'Article: Swapping bits and distributing hashes on the decentralized web (Textile)': https://medium.com/textileio/swapping-bits-and-distributing-hashes-on-the-decentralized-web-5da98a3507
  'GitHub repo: Go Bitswap implementation': https://github.com/ipfs/go-bitswap
  'GitHub repo: JavaScript Bitswap implementation': https://github.com/ipfs/js-ipfs-bitswap
---

# Bitswap

Bitswap是IPFS的一个核心模块，用于交换块数据。它指导了网络中与其他节点的块请求和发送。Bitswap是一种基于消息的协议，所有的消息都包含[期望列表](#want-list)或者块数据。Bitswap有一个[Go语言实现](https://github.com/ipfs/go-bitswap)和[JavaScript实现](https://github.com/ipfs/js-ipfs-bitswap)。

Bitswap有两个主要工作：

- 从网络中获取客户端请求的块。
- 向其他节点发送它拥有且其他节点需要的块。

## Bitswap如何工作

IPFS将文件分解成称为 _blocks_ 的块数据。这些块由[内容标识符(CID)](/concepts/content-addressing)来标识。运行Bitswap协议的节点想要获取一个文件时，它向其他节点发送期望列表（`want-lists`），一个`期望列表`是节点想接收的一组块CID的列表。每个节点都会记住其他节点想要的块，每次收到一个块时，它都会检查是否有节点需要此块，然后发送给需求方。

下面是一个简单版本的`期望列表`：

```javascript
Want-list {
  QmZtmD2qt6fJot32nabSP3CUjicnypEBz7bHVDhPQt9aAy, WANT,
  QmTudJSaoKxtbEnTddJ9vh8hbN84ZLVvD5pNpUaSbxwGoa, WANT,
  ...
}
```

#### 发现

为了找到有某个文件的节点，运行Bitswap协议的节点首先向所有连接上的节点发送一个名为 _want-have_ 的请求。该 _want-have_ 请求包含此文件的根块的CID（根块位于构成文件的块DAG的顶部）。拥有这个根块的节点发送一个 _have_ 的响应，并将其添加到一个会话中。其他没有此块的节点则发送一个 _dont-have_ 的响应。如果所有节点都没有此根块，Bitswap就查询分布式哈希表（DHT）以查询谁能够提供此根块。

![Diagram of the _want-have/want-block_ process.](./images/bitswap/diagram-of-the-want-have-want-block-process.png =740x537)

#### 传输

在节点添加到会话中之后，对客户端想要的每个块数据，Bitswap会发送 _want-have_ 请求到每个会话节点，以找到哪些节点有这个块。节点会响应 _have_ 或者 _dont_have_。Bitswap构建一个了一个映射表，包含节点和其拥有的块信息的映射关系。Bitswap发送 _want-block_ 到哪些拥有块的节点，然后它们会响应这些块数据。如果没有节点有这些块数据，Bitswap会向DHT发起查询，以找到能提供块数据的节点。

### 更多引用

- [February 2020: New improvements to IPFS Bitswap](https://blog.ipfs.io/2020-02-14-improved-bitswap-for-container-distribution/)
- [Technical overview of the Go implementation of Bitswap](https://docs.google.com/presentation/d/1mbFFGIIKNvboHyLn-k26egOSWkt9nXjlNbxpmCEQfqQ/edit#slide=id.p)
- [Article: Swapping bits and distributing hashes on the decentralized web (Textile)](https://medium.com/textileio/swapping-bits-and-distributing-hashes-on-the-decentralized-web-5da98a3507)
- "About Bitswap" Go implementation poster from the IPFS developer summit in Berlin in July 2018:
  !['About Bitswap' poster](https://user-images.githubusercontent.com/74178/43230914-f818dab2-901e-11e8-876b-73ba6a084f76.jpg 'Bitswap-Poster_Berlin-July-2018')
