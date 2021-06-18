---
title: 命令行快速启动
legacyUrl: https://docs.ipfs.io/introduction/usage/
description: Quick-start guide for installing and getting started with IPFS from the command line.
---

# 命令行快速启动

如果你精通命令行，并且只想立即启动和运行IPFS，按此教程执行。注意本教程假设你已经安装了go-ipfs，用Go语言编写的参考实现。

::: 提示
当前还不想使用命令行的话，可以尝试桌面应用的实现。它会自动完成本页面列出的各个步骤，因此你随时也可以在终端控制台中运行IPFS。[立即下载IPFS Desktop](https://github.com/ipfs-shipyard/ipfs-desktop)
:::

## 先决条件

如果还没有安装Go-IPFS，按此[安装说明](../../install/command-line)。

## 初始化仓库

`ipfs`把它的所有设置和内部数据保存在一个名为 _the repository_ 的目录。首次使用IPFS之前，需要用`ipfs init`命令初始化该仓库：

```bash
ipfs init

> initializing ipfs node at /Users/jbenet/.ipfs
> generating 2048-bit RSA keypair...done
> peer identity: Qmcpo2iLBikrdf1d6QU6vXuNb6P7hwrbNPW9kLAH8eG67z
> to get started, enter:
>
>   ipfs cat /ipfs/QmYwAPJzv5CZsnA625s3Xf2nemtYgPpHdWEz79ojWnPbdG/readme
```

如果是在数据中心的服务器上运行，应当使用`server`配置项来初始化IPFS。这样能够防止IPFS创建大量试图发现本地节点的内部流量：

```bash
ipfs init --profile server
```

还可以配置许多其他的配置选项，在[完整参考](https://github.com/ipfs/go-ipfs/blob/master/docs/config.md)中确认其他的配置项。

在`peer identity:`之后的hash值是你的节点ID，这个值和上面显示的不会一样。网络中其他的节点通过此ID来发现和连接到该节点。随时可以通过运行`ipfs id`命令来查看此ID。

现在可以尝试运行`ipfs init`输出信息中推荐的命令，类似于`ipfs cat /ipfs/<HASH>/readme`。

可以看到类似如下的结果：

```
Hello and Welcome to IPFS!

██╗██████╗ ███████╗███████╗
██║██╔══██╗██╔════╝██╔════╝
██║██████╔╝█████╗  ███████╗
██║██╔═══╝ ██╔══╝  ╚════██║
██║██║     ██║     ███████║
╚═╝╚═╝     ╚═╝     ╚══════╝

If you see this, you have successfully installed
IPFS and are now interfacing with the ipfs merkledag!

 -------------------------------------------------------
| Warning:                                              |
|   This is alpha software. use at your own discretion! |
|   Much is missing or lacking polish. There are bugs.  |
|   Not yet secure. Read the security notes for more.   |
 -------------------------------------------------------

Check out some of the other files in this directory:

  ./about
  ./help
  ./quick-start     <-- usage examples
  ./readme          <-- this file
  ./security-notes
```

还可以继续浏览仓库中的其他对象。尤其可以浏览`quick-start`，这个会显示其他可以尝试的命令示例：

```bash
ipfs cat /ipfs/QmYwAPJzv5CZsnA625s3Xf2nemtYgPpHdWEz79ojWnPbdG/quick-start
```

## 节点上线

在准备好让节点加入到公共网络中时，在另一个终端中运行ipfs守护进程，等到下面的三行信息出现，说明你的节点已经就绪了：

```bash
ipfs daemon

> Initializing daemon...
> API server listening on /ip4/127.0.0.1/tcp/5001
> Gateway server listening on /ip4/127.0.0.1/tcp/8080
```

记下这里显示的端口号，如果和上面不一样，在以下的命令中，需要将端口指定为所收到的值。

然后切换为原来的终端，如果你已经连接到网络中，应当可以运行以下命令看到你的节点中的IPFS地址信息：

```bash
ipfs swarm peers

> /ip4/104.131.131.82/tcp/4001/p2p/QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ
> /ip4/104.236.151.122/tcp/4001/p2p/QmSoLju6m7xTh3DuokvT3886QRYqxAzb1kShaanJgW36yx
> /ip4/134.121.64.93/tcp/1035/p2p/QmWHyrPWQnsz1wxHR219ooJDYTvxJPyZuDUPSDpdsAovN5
> /ip4/178.62.8.190/tcp/4002/p2p/QmdXzZ25cyzSF99csCQmmPZ1NTbWTe8qtKFaZKpZQPdTFB
```

这些地址的组合形式为：`<transport address>/p2p/<hash-of-public-key>`。

现在你可以从网络中获取对象数据了，尝试：

```bash
ipfs cat /ipfs/QmSgvgwxZGaBLqkGyWemEDqikCqU52XxsYLKtdy3vGZ8uq > ~/Desktop/spaceship-launch.jpg
```

以上命令让IPFS在网络中搜索CID为`QmSgv...`的内容，并将数据写到你的桌面上一个名为`spaceship-launch.jpg`的文件中。

下一步，可以尝试发送对象到网络中，然后在你的浏览器中查看。以下示例以`curl`模拟浏览器请求，你也可以直接在浏览器中打开对应的IPFS URL：

```bash
hash=`echo "I <3 IPFS -$(whoami)" | ipfs add -q`
curl "https://ipfs.io/ipfs/$hash"

> I <3 IPFS -<your username>
```

很不错吧？这时IPFS网关已经从你的计算机中获取了一个文件。网关首先查询了分布式哈希表(DHT)，找到你的主机，然后请求了对应的文件，你的计算机把文件发送给网关，然后网关把这个文件返回给你的浏览器。

根据网络状况的不同，`curl`可能需要花费一段时间。公共网关也可能超载，或者很难联系到你的计算机。

也可以通过你的本地网关来获取该文件：

```bash
curl "http://127.0.0.1:8080/ipfs/$hash"

> I <3 IPFS -<your username>
```

默认情况下，你的网关并不对外公开，它仅提供本地服务。

## Web控制台

可以打开`localhost:5001/webui`来查看你本地节点的web控制台。这个控制台应该类似下面这样：

![Web console connection view](./images/command-line-quick-start/webui-connection.png)

## IPFS伴侣

如我们所可以看到的，[IPFS伴侣](https://github.com/ipfs-shipyard/ipfs-companion#ipfs-companion)是一个浏览器扩展程序，可以简化我们对IPFS资源的访问，并扩展了浏览器对IPFS协议的支持。

它能够自动将对IPFS网关的请求转发到我们的本地守护进程中，这样你就不需要依赖或者信任远程的网关。

它可以在Firefox(桌面版和安卓版)和各类Chromiumn内核的浏览器中运行，如Google Chrome或者[Brave](https://brave.com)。
[查看其特性](https://github.com/ipfs-shipyard/ipfs-companion#features)并立即安装吧！

- [直接下载](https://github.com/ipfs-shipyard/ipfs-companion#install)
- [从Firefox附加组件中安装](https://addons.mozilla.org/firefox/addon/ipfs-companion/)
- [从Chrome应用商店中安装](https://chrome.google.com/webstore/detail/ipfs-companion/nibjojkomfdiaoajekhjakgkdhaomnch)

## 常见故障

### 检查Go版本

IPFS需要Go 1.12.0及以上版本。可以运行`go version`来查看其版本：

```bash
go version

> go version go1.12.2 linux/amd64
```

如果需要更新，推荐从[canonical Go packages](https://golang.org/doc/install)安装。包管理器中的Go包通常都过期了。

### 检查FUSE是否已安装

如果要挂载文件系统，需要安装和配置FUSE。更多如何配置FUSE的信息，可以参考[github.com/ipfs/go-ipfs/blob/master/docs/fuse.md](https://github.com/ipfs/go-ipfs/blob/master/docs/fuse.md)

### 更多帮助

IPFS社区非常友好，也很愿意提供帮助！可以在官方[IPFS论坛](https://discuss.ipfs.io/)中获取其他IPFS开发者的支持，或者在[Matrix](/community/chat/)中加入交流讨论。
