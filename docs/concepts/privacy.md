---
title: 隐私
sidebarDepth: 0
description: Learn about user privacy in IPFS and why it does not come with a built-in privacy layer or encryption.
related:
  'Article: IPFS Privacy (Textile)': https://medium.com/pinata/ipfs-privacy-711f4b72b2ea
---

# IPFS与隐私

作为一个点对点的数据存储和传输协议，IPFS是为公开网络而设计的：参与网络的节点所存储的数据隶属于全局唯一的[内容地址](/concepts/content-addressing) (CID)，并且通过公开可查阅的[分布式哈希表](/concepts/dht/) (DHTs)来发布他们所拥有的可供其他节点使用的CID。这个范式是IPFS的核心竞争力之一，从基础本质上来说，IPFS是一个管理着网络中所有可用数据的全球分布式服务器，可以同时被引用为内容本身（这些CID）和这些持有内容或者获取内容的参与者（节点）。

然而，这也意味着IPFS本身不会明确去保护关于CID和提供/检索他们的节点的信息。这并非是分布式网络所独有的，在分布式网络和经典网络中，流量和其他的元数据都可以被监控，并以此推断出许多关于其网络和用户的信息。以下简短概括了其关键细节：虽然IPFS节点间的流量是加密的，但他们发布到DHT中的元数据是公开的。节点会发布各种对DHT的功能来说很重要的信息，包括它们的唯一节点标识符（节点ID），它们所提供数据的CID，因此关于哪些节点正在检索和/或重新提供哪些CID的信息是公开可用的。

那么为什么IPFS协议自己没有明确内建一个“隐私层”呢？这符合协议的高度模块化设计的关键原则：毕竟在IPFS生命期内的不同用途可能会需要不同的隐私方案。在IPFS核心模块中明确实现一种隐私方案可能会因为在模块化、灵活性及未来适应性上的欠缺，从而限制后续的构建者。从另一方面来说，让人们可以根据针对当前场景自由的选择构建到IPFS中的最佳的隐私方案，也最大化了IPFS的适用性。

如果你在担心这个对你个人使用场景下的影响，可以采取额外的方案，如禁用内容重新提供、加密敏感内容甚至是在合适情况下运行一个专有的IPFS网络。以下会介绍详细的信息。

::: tip
节点间的IPFS流量是加密的，但是节点发布到DHT中的关键元数据是公开的，包括他们的唯一节点标识符（节点ID）和他们所提供数据的CID。如果你担心这个对你个人使用的影响，可以考虑采取额外的措施。
:::

## What's public on IPFS

所有的IPFS流量都是公开的，包括文件内容本身（除非它们被加密了，更多信息见下）。为了理解IPFS的隐私性，最简单的方式是从两个方面来看：内容标识符（CID）和IPFS节点本身。

### 内容标识符

因为IPFS使用的是[内容寻址](/concepts/content-addressing/)，而不是传统网络的位置寻址，存储于IPFS网络中的每一份数据都有一个自己唯一的内容标识符（CID）。与此CID相关联的数据副本可以存储在位于全球任意位置的不限数量的IPFS参与节点中。为了高效稳健的检索到一个与特定CID关联的数据，IPFS使用[分布式哈希表](/concepts/dht/) (DHT)来跟踪存储的内容。当通过IPFS来检索一个特定CID时，你的节点会查询DHT，以找到拥有该数据的最近的节点，同时默认情况下也会支持在一定时间内向其他节点重新提供此CID，直到定期的垃圾回收机制清空了一段时间内未被使用的内容缓存。你也可以固定那些明确不想被垃圾回收的CID，可以明确使用IPFS的底层固定API，也可以使用[Mutable File System](/concepts/file-systems/#mutable-file-system-mfs) (MFS)来间接实现，这也意味着你成为了该数据的永久提供者。

这是IPFS相对于传统网络托管的优势之一。这意味着检索文件，尤其是存储于大量网络节点中的流行文件，可以更快，也更节省流量。但是，需要知道很重要的一点：DHT查询是公开的，因此第三方有可能监视此流量，以确定哪些CID正在被请求，是何时由谁请求的。随着IPFS的日益普及，这种监控的可能性也在日益增加。

### 节点可标识性

IPFS流量监控需要关注的另一部分在于，节点自身的唯一标识符也是公开的。和CID一样，每个独立的IPFS节点也有自己的公开标识符（称为节点ID），类似于`QmRGgYP1P5bjgapLaShMVhGMSwGN9SfYG3CM2TfhpJ3igE`。

虽然这一长串文本数字的字符串不像"Johnny Appleseed"一样是易于被人阅读的文本，节点ID也是你节点的一个长期唯一的标识符。要记住的是可以通过对你的节点ID进行DHT查询，从而获取到你的IP地址，尤其是你的节点定期从同一位置（如你的家里）运行时。（可以在必要时[重置节点ID](https://docs.ipfs.io/reference/cli/#ipfs-key-rotate)，但如同在传统的网站APP和服务中修改用户名一样，这也会带来别的影响。）此外，对公共IPFS网络的长期监控，也可能生成关于你节点请求的和/或重新提供的CID及操作时间的信息。

## 增强IPFS隐私性

当存在需要保持隐私性，又需要使用IPFS的场景时，以下的任一方案可能有用。同时记住，你随时可以在官方[IPFS论坛](https://discuss.ipfs.io)中讨论隐私性，并获得他人的意见或想法。

### 控制共享内容

默认情况下，IPFS节点向网络宣称其会共享在缓存中的所有CID（换而言之，会重新发布它从其他节点检索到的内容），以及明确固定的或者添加在MFS中以使其始终可用的CID。如果要禁用该行为，可以在节点配置文件的[reprovider设置](https://github.com/ipfs/go-ipfs/blob/master/docs/config.md#reprovider)中修改。

修改reprovider设置为"pinned"或"roots"将使节点不再宣布自己会提供缓存中没有固定内容的CID，这样你仍然可以使用固定来向其他节点提供那些你关注的内容和期望在IPFS中确保持续可用的内容。

### Using a public gateway

使用公共[IPFS网关](/how-to/address-ipfs-on-web/#http-gateways)是一种不泄露你本地节点的任意信息，又可以获取IPFS托管内容的方式，因为你完全没有使用本地节点。然而此方法使你无法享受到完全参与到IPFS网络中带来的所有好处。

公共IPFS网关主要用于作为传统网络和分布式网络之间的桥梁；它们允许普通的网页客户端通过HTTP来请求IPFS托管的内容。这对向后兼容非常有用，但是如果你只是通过公共网关而非直接通过IPFS来获取内容，你并没有实际参与到IPFS网络中；网关才是网络的参与者，代表你行使了行为。另外一点也很重要：网关运营商也可以收集他们自己的私有指标，其中可能包括跟踪使用网关的IP地址，并将其与请求的CID相关联。此外，通过网关发出的内容请求在公共DHT上依然可见，只是不可能知道具体是谁发起的请求。

### 使用Tor

如果你熟悉[Tor](https://www.torproject.org/)以及命令行，可以考虑尝试配置节点设置，以[通过Tor传输协议来运行IPFS](https://dweb-primer.ipfs.io/avenues-for-access/tor-transport)。

如果你是一个IPFS上的开发人员，值得关注的是全球IPFS社区一直在尝试使用Tor传输协议，查看[this example from e-commerce organization OpenBazaar](https://github.com/OpenBazaar/go-onion-transport)，且可能已经有一个开源代码库供你在自己项目中来实现它了。

### 通过IPFS传输加密内容

如果你对隐私的顾虑更多在于IPFS提供的内容本身的可见性，而非潜在的行为监视，可以简单的通过对内容进行加密，然后再添加到IPFS网络中来缓解它。此时虽然关于加密内容的流量仍然可能被追踪，但是CID对应的加密数据对于那些无法解密的人来说，依然不可读。

这里有一个警告需要记住：虽然今天的加密手段现在看起来是万无一失的，但不代表未来也无法破解。未来计算能力的突破可能会允许返回过去，以解密放在公共网络如IPFS中的旧内容。如果你想防范这种潜在的攻击模式，可以使用IPFS混合专有网络，其中节点位于连接网关之后，这个网关在向一个节点转发请求之前，会检查请求的ACLs；这是一个潜在的设计方向。（[this article from Pinata](https://medium.com/pinata/dedicated-ipfs-networks-c692d53f938d) 可以帮助了解更多细节）

如果你对大规模实施IPFS加密特性感兴趣，可以阅读[this case study on Fleek, a fast-growing IPFS file hosting and delivery service](/concepts/case-study-fleek/).

### 创建一个专有网络

[专有IPFS网络](https://github.com/ipfs/go-ipfs/blob/v0.8.0/docs/experimental-features.md#private-networks)提供了全面的保护以杜绝公共监控，但同样也缺失了公共IPFS网络的规模优势。专有网络和公共网络的运作方式一致，但又一个关键区别：它只能够被已授权的节点访问，也只能在这些节点中扩展。这意味着公共IPFS网络的大规模优势，如地理位置的弹性和高需求内容的快速检索特性都无法实现，除非该专有网络也明确的为此针对性设计和延展。

对企业来说，运行专有网络是一个很好的IPFS方案实现选项，例如[this case study on Morpheus.Network](/concepts/case-study-morpheus/)，因为网络拓扑可以完全按需来设计和构建。
