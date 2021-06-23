---
title: 应用思路与实例
sidebarDepth: 0
description: Explore some helpful use cases, ideas, and examples for the InterPlanetary File System (IPFS).
---

# 应用思路与实例

IPFS是一个通用技术，可以在大量的场景中应用。以下是一个很长但远非详尽的基于IPFS的项目列表。有些是极简化的原型，有些则是成熟公司支持的完整项目。

## 共享文件

要掌握的最简单场景之一，就是在节点间共享文件。

### 桌面应用

[IPFS Desktop](https://github.com/ipfs-shipyard/ipfs-desktop)是官方的IPFS桌面客户端。自带IPFS节点，可以固定文件，并提供共享链接。这通常被认为是访问IPFS生态的最简单入口。

(LU:2019/01)[Arbore](https://arbo.re) Arbore是一个自由开源的文件共享应用，使你可以私密的，不受限制的把照片、文档和文件发送给你的联系人。

(LU:2020/09)[Orion](https://orion.siderus.io)是另一个选择。Orion是一个易用的IPFS桌面客户端。可以在没有任何命令行知识和技术背景的情况下帮助人们在公共网络上共享文件。该应用包含了启动个人IPFS节点所需的一切。

### 共享文件或者出售文件拷贝

[Enzypt.io](https://enzypt.io) 使用eth出售一个文件，或者只是生成一个共享链接。

(LU:2019/01)[FileNation](https://github.com/FileNation/FileNation)如果只想共享的话，是另一个选择。

### Dead drop

(LU:2016/03)[dead drop](https://deaddrops.com/)是一个U盘或者别的存储设备，它固定于某一公共场合，以供人们获取或者存放文件。拜[IPFS Dead Drop project](https://github.com/c-base/ipfs-deaddrop)所赐，现在有了一个IPFS的版本。

## 协作

让IPFS来协调你和同事之间的数据流，即便在离线时或者处于本地网络时也能实现。

### 协作处理文档

[PeerPad.net](https://peerpad.net)允许你在浏览器中直接编辑、协作和导出Markdown文档，类似于Google Docs的方式。

### 版本控制

(LU:2019/04)[星际版本控制系统(IPVC)](https://github.com/martindbp/ipvc)是一个构建于IPFS的分布式版本系统系统，类似于[git](https://git-scm.com/)。它适用于任何类型的数据，不仅仅是人类可读的内容。它也特别适用于大型文件的版本控制。其底层概念深受[git](https://git-scm.com/)和[gitless](https://gitless.com/)的影响。

### 连接事件参与者

[Gthr.io](https://gthr.io)是一个简单的demo，通过让参与者扫描彼此的二维码，将他们连接到一个事件中。这是为IPFS Camp 2019开发的，可以查看他们的[演示文稿](https://www.dropbox.com/s/wodmbi6ico3inya/Offline%20Presentation.pdf?dl=0)。这个简单应用的代码在其[GitHub仓库](https://github.com/JustMaier/gathering)中。

### 交流消息

[Berty.tech](https://berty.tech/)作为一个基于IPFS的消息应用，能够确保设备间无需通过服务器，直接建立连接。没有互联网连接时，它可以在本地网络中工作，同时还可以通过蓝牙或其他近场传输方案来工作。

Berty正在致力于将[IPFS应用于移动设备](https://github.com/ipfs-shipyard/gomobile-ipfs)。目前已经有了专业人士为IPFS在移动设备兴起所制定的一些[指导方针](https://jkosem.gitbook.io/ipfs-mobile-guidelines/)。

## 存储应用数据

通过将项目的小脚本或者大型数据库存储在IPFS中，根据具体架构，可以获得许多不同的好处。如果用户并没有使用IPFS客户端，你仍然开箱即用的使用了重复数据删除的功能。如果用户使用了IPFS客户端，他们同时还帮你提供了内容的对外提供服务，从而减轻了你自己的基础设施的负载，并在服务掉线时增加了其在线服务时间。用户也不会向你的服务器请求他们已有的数据内容。

### 去中心化虚拟现实

[Decentraland](https://decentraland.org/)是一个虚拟世界，可以通过VR装置、电脑或者智能手机来探索。他们的所有应用数据都存储于IPFS中，因此这些繁重的文件可以同时从其他许多用户中获取，从而能够更快的加载和同步。

### 视频托管平台

[DTube](https://d.tube)在IPFS中托管视频，以减轻他们基础设施的压力。此网站本身并非去中心化的，它主要用于管理协调用户，以及内容的可发现性。你可以了解如何[在IPFS中复制Youtube](https://simpleaswater.com/youtube-on-ipfs/)。

### 协作托管大型数据集

[Qri](https://qri.io/)是一个开源工具，用于管理大型数据集。Qri的用户享受了更低的托管成本，数据更改的可追溯性，版本可回滚以及更轻松的协作更新数据。数据集间的数据去重同样有助于尽量减少物理存储空间和同步所需时间。

### 大数据分析并行化

在一些繁重的分析任务中，可以受益于使用Hadoop等工具，在多个节点上进行并行计算。但是在非常大的数据集中，为每个节点获取运算相关子集所需的时间比实际运算的时间还要长。Scott Brisbane在一篇[论文](https://s3-ap-southeast-2.amazonaws.com/scott-brisbane-thesis/decentralising-big-data-processing.pdf)中提出了一个设计，通过使用IPFS能够大幅降低获取子集的时间，并使中分析时长降为原来的1/4。这里是此概念的[一页摘要](https://www.cse.unsw.edu.au/~hpaik/thesis/showcases/16s2/scott_brisbane.pdf)。

### 死亡开关

[Killcord](https://killcord.io/)是一个开放项目，如果用户在一定时间内没有签到，会自动公开发布数据。这可以保证如某个记者正在进行的调查能够被继续下去，即便他们自己已经无法开展调查工作，因为所有收集到的信息现在都被公开了。

### IPFS地图服务

已有一个[IPFS捐赠](https://github.com/ipfs/devgrants)，用于[在IPFS中托管OpenStreetMaps数据](https://github.com/ipfs/devgrants/blob/8233f7df4a219122bcf31eaea289d654406e4443/targeted-grants/open-street-map-ipfs.md)。从长远来看，这意味着使用该方案的应用能够更快的同步数据，对其服务器的带宽需求也更低。

### P2P视频流平台

[Blust.tv](http://www.blust.tv/)期望在用户请求电影时能通过IPFS来分发。通过加入他们专有的密钥信息，可以促进P2P网络中的合法视频数据流。

### 帮助托管重要数据

由于最近IPFS集群的更新，现在可以请求他人来帮助存储数据，而不用信任其不会修改数据。使用[协作模式](https://cluster.ipfs.io/documentation/collaborative)，可以无需了解IPFS的复杂性，从而[复制Pacman包或者COVID-19相关的论文](https://collab.ipfscluster.io/)。

### 视频直播

让用户之间可以无需通过服务器，相互之间进行流式传输，从而不会使现有流行的流媒体平台过载。可以从正在开发的原型[Toronto Mesh](https://github.com/tomeshnet/ipfs-live-streaming)，或者[Fission](https://fission.codes)的[试验](https://blog.fission.codes/experimenting-with-hls-video-streaming-and-ipfs/)中获取灵感。[Fleek](https://fleek.co)也使用他们的Amazon S3/IPFS 兼容性工具做了一些[试验](https://medium.com/temporal-cloud/introducing-s3x-endless-ipfs-dynamic-possibilities-stream-videos-host-dynamic-websites-f0072127070f)。

### 去中心化机器人

机器人公司[KODA](https://www.koda9.com/)正在开发全球首个去中心化的机器狗，名为[Koda-9](https://www.whipsaw.com/thinking/new-era-of-household-robots/)。它使用IPFS来存储用户数据，如安全录像等。

## IPFS作为基础设施

使用IPFS可以让你从许多机器间协调的复杂性中脱离出来。无论你的架构如何，IPFS都可以开箱即用式的处理负载均衡、数据去重、缓存和高可用性保障。其高模块化设计也使你可以很方便的对其进行定制。

### 去中心化集群

[IPFS Cluster](https://cluster.ipfs.io/)是管理集群节点以复制数据的官方工具。和其他分布式集群一样，你将获得在荣誉、负载均衡和写入权限管理方面的好处。可以选择将集群连接到其他IPFS网络中，或者使其专有运行。也可以基于[协作模式](https://cluster.ipfs.io/documentation/collaborative/)，邀请外部人员来帮助复制数据，但不授予写入权限。

### 内容分发网络

[内容分发网络](https://en.wikipedia.org/wiki/Content_delivery_network) (CDN)是一个在物理距离上离用户很近的节点上存储内容的节点网络。与用户物理距离很近的服务器可以确保低延迟、启用负载均衡，并允许按需进行数据内容可用性的扩展。IPFS网络就是按照CDN设计的，因为每个节点都会缓存他们需要的内容，并向其他节点提供。

### 分布式包管理器Distributed package managers

[Node package manager (NPM)](https://www.npmjs.com/)具有IPFS中的镜像。使用特定的客户端[npm-on-ipfs](https://github.com/ipfs-shipyard/npm-on-ipfs)，就可以从IPFS中获取包，并分发给其他需要它们的客户端。例如，在同一栋建筑中的团队就可以彼此间获取包，从而减少公司的流量费用。

[Nix package manager](https://nixos.org/)的开发者们正在集成IPFS，以分发二进制包和源码。

### 托管软件容器

Netflix正在使用IPFS来[全球同步其Docker容器](https://blog.ipfs.io/2020-02-14-improved-bitswap-for-container-distribution/)。由于每个节点都从他们所知最快的节点中获取数据，同步要比常规方式快很多。

### Efficient network factories

[Actyx](https://www.actyx.com/)正在帮助生产制造商来升级他们的工厂到[工业4.0](https://en.wikipedia.org/wiki/Industry_4.0)时代，这意味着要把机器连接在起来，以获得更好的性能、故障容忍度和灵活性。Actyx在所有的机器上部署了构建于IPFS之上的一个定制化的操作系统，它们能发送指标，接收指令，同步信息，并在本地计算下一步的行动。

## 降低存储使用率

通过在节点中只存储一份相同的数据，IPFS也是存储受限项目的一个自然选择。

### 压缩遥测数据

传感器数据通常都是结构化的，包含重复的数据块。在需要记录大量传感器数据的情形下，[通过重复数据删除技术](http://blog.klaehn.org/2018/06/10/efficient-telemetry-storage-on-ipfs/)，IPFS甚至可以比简单压缩还要多的降低存储使用量。

## 数据去中心化

通过将数据去中心化，可以提高其可用性，以防止服务器出现故障、ISP对你发布的内容不满意或者敌对政府发出了删除指令。同时还能够减少保持连接的用户的数据加载时间，并使应用在本地脱机时或本地网络中可以工作。取决于具体应用，通过本地去重和缓存，你的用户还可能减少存储需求和下载时间。

### 去中心化数据库

[OrbitDB](https://github.com/orbitdb/orbit-db)是一个无服务器、分布式的点对点数据库。OrbitDB使用IPFS作为数据存储，使用IPFS Pubsub来与节点自动同步数据库。这是一个最终一致的数据库，使用CRDTs来进行无冲突数据合并，使得OrbitDB成为去中心化应用（dApp）、区块链应用和离线优先Web应用的绝佳选择。当前具备Go和Javascript语言的可用实现。

如果当前正在使用MongoDB，可能会更喜欢[ThreadDB](https://docs.textile.io/threads/introduction/)或[AvionDB](https://github.com/dappkit/aviondb-onboard)，它们是构建于IPFS的非结构化数据库。

### 使用Textile进行IPFS托管

[Textile](https://Textile.io)是一个托管公司，依托于IPFS之上开发了新的IPFS服务层。除此之外，他们还提出了独立的云环境，称之为[buckets](https://docs.textile.io/hub/buckets/)。Textile还开发了令人影响深刻的工具集，用于[构建去中心化应用及集成](https://blog.textile.io/announcing-the-textile-protocol-hub/)。

### NextCloud集成

JusticeNode已经为流行的自托管云服务NextCloud开发了一个[扩展](https://github.com/justicenode/files_external_ipfs)。该集成允许用户使用IPFS作为外部存储。

### 在IPFS中部署网站

[Fleek.co](https://fleek.co/)允许你在IPFS上轻松的构建网站和应用。工作流类似于Netlify：开发者可以把他们在GitHub中的网站或web应用链接到Fleek中，然后特定分支更新的时候，Fleek会自动构建并更新部署到IPFS中。Fleek还提供了以太坊名字服务（ENS）和域名集成，还计划支持更多的部署方式：更多的Git服务商，通过命令行接口部署，拖拽目录，通过API部署等。

### Ethereum与Solidity相关应用

[Embark](https://framework.embarklabs.io/)是一个用于开发和部署去中心化应用的一站式开发者平台。当前已经与以太坊区块链、去中心化存储如IPFS，以及去中心化社区平台如Whisper和Orbit进行了集成。

### Static-site generators

Several plugins exist to decentralize your website built with popular static-site generators like [VuePress](https://github.com/cwaring/vuepress-plugin-ipfs) or [Gatsby](https://github.com/moxystudio/gatsby-plugin-ipfs).

## 构建DAPP

有很多有用的框架可以用来在IPFS中构建去中心化应用。例如[Dappkit](https://dappkit.io/), [Fission](https://fission.codes/), [Fleek](https://fleek.co/), 或者[Textile](https://textile.io/)。

### SecureMyState

这个用于政府和公民交流的应用是在[DenverETH 2020](https://ethdenver.com/)黑客马拉松期间，2天内构建的。它允许科罗拉多州的公民管理他们自己的州有数据，如他们的驾照状态或商务登记信息。可以在[Medium](https://medium.com/twos-complement/secure-mystate-government-services-for-citizens-ethdenver2020-17f47407b656)上找到这个项目的更多信息。

### Marketplace

[Origin](https://www.originprotocol.com/)是一个基于区块链的在线商务平台，其数据存储于IPFS中。[Brave](https://brave.com/)浏览器在这里有它自己的商店，以出售相关的物品。

### Torrent tracker hub

[BitTorrent](https://en.wikipedia.org/wiki/Bittorrent)是一个强大的P2P文件共享技术，在有中心化的追踪节点来帮助用户获知谁拥有哪些数据时，它可以运行的更好。有些人也在尝试将其去中心化，可以[在此](https://github.com/urbanguacamole/torrent-paradise)找到他们的原型工作。

### COVID-19追踪器

这个简单的追踪API允许任意IPFS节点请求COVID-19大流行的最新数据。在[GitHub](https://github.com/RTradeLtd/ipcoronafs)查看其代码。

### GitHub集成

[GitHub Action](https://github.com/marketplace/actions/upload-to-ipfs)让你可以在IPFS上自动上传GitHub发布版本。

### 备份Wolfram数据

在最近一次更新中，Wolfram允许用户在IPFS中保存他们的计算和资产。查看[版本说明](https://writings.stephenwolfram.com/2020/03/in-less-than-a-year-so-much-new-launching-version-12-1-of-wolfram-language-mathematica/)以了解更多信息。

### 音乐流媒体平台

[Audius](https://audius.co/)是一个构建于IPFS的音乐流媒体平台。艺术家可以完全控制它，平台不收取任何费用，听众在离线时也可以欣赏音乐。

### 音乐播放器

[Diffuse.sh](https://diffuse.sh/)是一个在线音乐播放器，你可以连接到你的音乐库，以从任何地方收听。现在它也可以连接到IPFS音乐库上。

### 去中心化自治组织(DAO)

[Aragon](https://aragon.org/)，一个用于创建DAO的服务，使用IPFS来存储数据。

## 去中心化自身网络

互联网网络的一些核心部分依然是中心化的，使其更容易被破坏或被审查。IPFS可以使网络本身更具备弹性。

### 去中心化DNS

[域名服务](https://en.wikipedia.org/wiki/Domain_Name_System) (DNS)是互联网中最中心化的部分之一。因为必须向一个中心节点请求如何访问到`google.com`或`facebook.com`，导致它也成为故障的集中点。在IPFS中备份DNS可以改进它的可用性，一个JavaScript的原型已经[发布在NPM中](https://www.npmjs.com/package/orbitdns)。

### 网络存档

基于重复数据删除方案，IPFS也是一个强大的网络归档工具。[InterPlanetary Wayback](https://github.com/oduwsdl/ipwb)正在进行这项工作的努力。

### 抗击审查

在土耳其维基已经很多年不允许访问了。IPFS背后的公司Protocol Labs，正在IPFS中托管一个维基镜像。更多信息可以查看原始的[博客](https://blog.ipfs.io/24-uncensorable-wikipedia/)和[项目代码](https://github.com/ipfs/distributed-wikipedia-mirror)。

## 区块链应用场景

IPFS非常适用于[区块链](https://en.wikipedia.org/wiki/Blockchain)的场景下。参与者之间的公共状态信息分布在区块链上，具体的数据则存储在IPFS中。基于内容寻址，区块链上只要存储IPFS的多重哈希，用户就可以从任意节点中获取到对应的正确数据。这种架构正在成为区块链应用的事实标准。

### 全球数据存储市场

[Filecoin](https://filecoin.io/)允许任何存储提供方为需要额外空间的用户托管其数据。它今年稍晚的时候会完全上线，这是IPFS一开始设计时的想法之一，也是促进IPFS增长的巨大加速器。该项目由[Protocol Labs](https://protocol.ai/)开发。

Textile.io正在构建[Powergate](https://blog.textile.io/filecoin-developer-tools-concepts/)工具，使得一旦主网发布，应用程序就可以与Filecoin进行交互。

### 物联网数据交换

[IOTA](https://www.iota.org/)是一个基础设施，用于维护Tangle，一个类似于区块链的零费用网络。它们的愿景是使得传感器、机器及其他设备之间能自动化交换数据，以免费或者运营商收费的方式。IOTA[宣称](https://docs.iota.org/docs/blueprints/0.1/tangle-data-storage/overview)这些数据已经可以存储于IPFS中，在[这里可以查看demo](https://ipfs.iota.org/)。

### 将加密信息转为人类可读的地址

[Humanize Pay项目](https://devfolio.co/submissions/humanize-pay)使人们拥有一个人类可读的以太坊地址，这样用户就无需处理冗长且难以记住的地址了。

### AI即服务

[MindSync](https://mindsync.ai/)期望构建一个托管AI竞争能力的区块链，从而满足AI技能的供需。

### 教育平台

[RocketShoes](https://www.rocketshoes.io/)是一个教育平台，学生在此可以制作和标记学习资料、作业、笔记和其他数字资产。这些内容都存储于IPFS中，然后通过区块链来提供时间戳信息、所有权证明和激励层保障。

### 在多个区块链中查询DWeb信息

[TheGraph](https://thegraph.com/)期望使用户可以在任意区块链和分布式服务中查询信息。其技术依赖于GraphQL来实现查询语句，以及IPFS来提供链索引信息。

### 所有权证明

通过IPFS和区块链，可以无需披露文件即可证明在特定时刻对此文件的所有权。这里有一个[实现案例](https://github.com/mustafarefaey/PrivateStamp)。

### 人类身份证明

[Idena](https://idena.io/)是一个用于证明你是人类（与DID有所不同）的区块链。它底层使用了IPFS。

### 构建智慧城市应用

[Robonomics](https://robonomics.network/)正在Ethereum和IPFS之上构建一个框架，以使得[智慧城市](https://en.wikipedia.org/wiki/Smart_city)应用之间更容易交换数据（如来自传感器或计算结果的数据）。

### 去中心化预测市场

[Augur](https://www.augur.net/)是一个区块链上的去中心化市场。你可以在此押注任何事情，或请求大众智慧来预测事情。如许多区块链应用一样，其数据也是托管在IPFS中。

### 去中心化气象数据

[Arbol](https://www.arbolmarket.com/)是一个气象风险市场，依赖区块链技术来追踪和验证气象数据。Arbol使用IPFS来存储数TB的已验证数据，而无需担心数据被篡改。

## 去中心化身份标识

[去中心化身份标识](https://en.wikipedia.org/wiki/Decentralized_Identifiers)是这样一个概念：将个人数据都存储于个人设备中，而不是在每个使用的服务中保存其中的一部分副本。你对自己的数据有控制权，即你可以决定哪些应用有权访问哪些数据；只需要对个人信息填充一次；允许撤销数据授权；去中心化身份标识是一个热门话题，IPFS则是其核心技术之一，许多工程师正在围绕它来构建系统。身份标识钱包[Nomios](https://nomios.io/)在[IPFS Camp 2019](https://github.com/ipfs/camp)中分享了关于去中心化身份标识通用的[一些想法](https://docs.google.com/presentation/d/1HbydOI0w-T_FY23zCACAyHmzDq1ZvyG2tklpPSm6OQQ/edit#slide=id.g5c88e8f60d_0_11)。

### Element

[Element](https://element-did.com)是一个开源项目，将内容寻址和Ethereum的智能合约的可交互能力结合起来，提供了一个身份标识管理工具。

### 3ID Connect

3ID Connect由[3Box](https://3box.io/)开发，是一个向基于IPFS开发的应用提供的个人数据管理器。3Box期望如同点击“连接到Google”或者“连接到Facebook”一样简单的来使用去中心化身份标识。可以阅读他们的[文章](https://medium.com/3box/introducing-3id-connect-531af4f84d3f)，其中说明了如何将其集成到你的应用中。

### Microsoft ION

[Microsoft](https://www.microsoft.com/)也开始试验将一些去中心化身份标识信息固定到IPFS中，然后把它们的hash值发布到比特币区块链中。Microsoft在一篇[博客中发布了相关的发现](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/toward-scalable-decentralized-identifier-systems/ba-p/560168)。

### Ceramic协议

[Ceramic协议](https://www.ceramic.network/)是IPFS中的另一个去中心化身份方案提议，具备完整的数据和文档交换协议。

## Non-implemented use cases

Here is a non-exhaustive list of use cases that were not implemented yet. Pick up the challenge yourself or follow your own idea! If you want to discuss your idea or have some problems, head to [the IPFS forum](https://discuss.ipfs.io) or [the IPFS help page](https://ipfs.io/help/).

- [Coordinate activists groups without fear from censorship](https://discuss.ipfs.io/t/building-a-secure-activist-membership-management-tool-on-ipfs/5702/3)
- [Manage the knowledge of the whole world](https://www.underlay.org/)
- [Build a giant database for genetic data](https://discuss.ipfs.io/t/addressing-petabytes-of-genetic-data-with-ipfs/1471/10)
- [Create a mesh network on IPFS](https://discuss.ipfs.io/t/mozilla-nsf-a-2-million-prize-to-decentralize-the-web/654)
- [Bring additional services to your existing mesh network](https://www.nycmesh.net)
- Serverless online gaming
- [Put Hadoop on IPFS](https://discuss.ipfs.io/t/minerva-build-the-hadoop-hive-on-ipfs/5832)
- Add more transports to IPFS for mobile usage ([here](https://github.com/ggerganov/wave-share#wave-share) or [here](https://github.com/RTradeLtd/libp2p-lora-transport) )
- [Cohost the site you visit](https://github.com/ipfs-shipyard/cohosting)
- [Distributed cards for spaced learning](https://discuss.ipfs.io/t/check-out-my-flashcard-app-that-uses-the-ipfs/6543/2)
- [Unblock the offline-first use cases](http://offlinefirst.org/casestudies/)
- [Build a distributed OS](https://github.com/ipfs/ipfs/issues/247)
- [Distribute the medical data](https://github.com/ipfs/notes/issues/292)
- Improve information sharing during disaster recovery
- [Get a grant for your help or propose an open grand](https://github.com/ipfs/devgrants)
- Make an interactive app for your classroom
- [构建一个分布式的社交媒体](https://discuss.ipfs.io/t/social-media-architecture-with-ipfs/4625/58)
- [学术论文的去中心化同行评审](https://discuss.ipfs.io/t/ipfs-implementation-for-publication-of-decentralized-peer-review/7692)
- [帮助托管NASA数据库](https://discuss.ipfs.io/t/nasas-earthdata-cloud/7528)
- [帮助托管LibGen材料](https://discuss.ipfs.io/t/torrent-site-of-scientific-papers-needs-help/6815)
- [构建一个漫画阅读器](https://discuss.ipfs.io/t/manga-reader-over-ipfs/4832)
- [构建一个Gif分享平台](https://discuss.ipfs.io/t/a-vine-clone-built-using-ipfs/3337)
- An interactive P2P application working in crowded events such as festivals
- [IPFS上的Ethereum/Solidity智能合约CI工具链](https://discuss.ipfs.io/t/ethereum-solidity-smart-contract-ci-toolchain-on-ipfs-wip/1780)
- [来自GitHub的各种想法](https://github.com/ipfs/ipfs/labels/applications%20of%20ipfs)
