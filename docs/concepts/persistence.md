---
title: 持久性
legacyUrl: https://docs.ipfs.io/guides/concepts/pinning/
description: Learn about how IPFS treats persistence and permanence on the web and how pinning can help keep data from being discarded.
---

# 持久性 Persistence, 永久性 permanence, 与固定 pinning

理解IPFS固定背后的概念，以及持久性 Persistence, 永久性 permanence, 与固定 pinning之间的区别。

## 持久性 Persistence 与 永久性 permanence

IPFS的一个目标就是通过让用户存储数据的同时，最大程度降低数据丢失或意外删除的风险，从而保留人类的历史。这通常被称为永久性（permanence）。但是永久性到底意味着什么，为什么很重要呢？

一项2011年的研究表明，[网页的平均生命周期是100天](https://blogs.loc.gov/thesignal/2011/11/the-average-lifespan-of-a-webpage/)，之后它就永远消失了。我们这个时代的主要媒介是如此脆弱，这并不是理想的状态。IPFS可以保留你期望存储的文件的每一个版本，并且可以很容易的配置弹性网络以进行数据镜像。

IPFS网络的节点能够自动缓存他们下载的资源，并且对其他节点保持这些资源的可用性。该系统的运作依赖于节点愿意且能够为网络缓存和共享资源。存储是有限的，因此节点需要清除一些之前的缓存资源，以为新资源腾出空间，这个过程称为垃圾回收。

为了确保数据在IPFS上的持久存在，不会被垃圾回收所删除，[数据可以被固定](/how-to/pin-files/)到一个或多个IPFS节点中。固定让你在磁盘空间和数据保留间可以控制选择。因此你可以控制固定你希望在IPFS中无限期保留的任何内容。

## 垃圾回收

[垃圾回收](<https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)>)是一个广泛用于软件开发中的自动资源管理方式。辣鸡回收器会尝试收回不在使用的对象所占用的内存。IPFS使用垃圾回收来删除它认为不再需要的数据，以回收磁盘空间。

IPFS垃圾回收器在[go-ipfs配置文件](https://github.com/ipfs/go-ipfs/blob/master/docs/config.md)的`Datastore`部分进行配置。与之相关的重要配置有：

- `StorageGCWatermark`：`StorageMax`的百分比值，如果守护进程启用了自动垃圾回收，在此值之上垃圾回收会被自动触发。默认值是`90`。

- `GCPeriod`: 指定垃圾回收的执行频率，在启用自动垃圾回收时生效。默认值是1小时。

要手动启动垃圾回收，[运行`ipfs repo gc`](https://docs.ipfs.io/reference/cli/#ipfs-repo-gc)：

```bash
ipfs repo gc

> removed QmPZhyTu8D7NqR5NvgkgNYsSYD4CNjnyuFejB8i23itJvA
> removed QmSYQFVAZgEnpa6NxiW5agyj3XU9VR4CbERShXiLhuPPPE
> removed QmS6SJXApoi59hqD8Naktgakc6UNHK1XDhqhtMg9sBhY8g
```

要启动自动垃圾回收，在启动IPFS守护进程时，使用`--enable-gc`标志：

```bash
ipfs daemon --enable-gc

> Initializing daemon...
> go-ipfs version: 0.8.0
> Repo version: 10
> ...
```

::: tip
如果使用IPFS Desktop，可以单击IPFS Desktop的任务栏图标，然后选择**Advanced** → **Run Garbage Collector**。
:::

## Pinning in context

IPFS节点可以根据不同类型的用户事件来保护数据不被垃圾回收。
-通用的方式是添加一个底层的[本地固定](/how-to/pin-files/)。这个对所有数据类型有效，可以手动执行。如果你通过CLI命令[`ipfs add`](/reference/cli/#ipfs-add)添加了一个文件，IPFS节点会自动为你固定这个文件。
- 在处理多个文件及目录时，更好的方式是将他们添加到本地的[可变文件系统(MFS)](/concepts/glossary/#mfs)中。这个以和本地固定一样的方式来保护数据不被垃圾回收，而且更容易管理。

::: tip
如果你想了解固定是如何与IPFS数据的整个生命周期相适配，阅读这个课程[IPFS Camp _The Lifecycle of Data in DWeb_](https://www.youtube.com/watch?v=fLUq0RkiTBA)。
:::


## 固定服务

为确保你的重要数据能够被保留，可以考虑使用固定服务。这些服务运行了大量节点，允许用户付费将数据固定在上面。一些服务还对新用户提供了免费的存储空间。固定服务适用于以下场景：

- 你没有很多磁盘空间，但想确保数据始终存在。
- 你的计算机是笔记本电脑、手机或者平板，会间歇性连接到网络中。但是你希望能够随时随地访问IPFS中的数据，无论你添加数据的设备是否在线。
- 你希望在不小心删除或者垃圾回收了自己计算机上的数据时，仍有一个备份，能够通过网络中的其他计算机来确保数据总是可用的。

一些可用的固定服务提供商如下：

- [Axel](https://www.axel.org/blog/2019/07/23/qa-with-the-developers-of-axel-ipfs/)
- [Eternum](https://www.eternum.io/)
- [Infura](https://infura.io/)
- [Pinata](https://pinata.cloud/)
- [Temporal](https://temporal.cloud/)
- [Crust Network](https://crust.network/)

查看如何[使用远程固定服务](/how-to/work-with-pinning-services/).
