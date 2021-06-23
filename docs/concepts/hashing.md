---
title: 哈希
legacyUrl: https://docs.ipfs.io/guides/concepts/hashes/
description: Learn about cryptographic hashes and why they're critical to how IPFS, the InterPlanetary File System, works.
---

# 哈希

::: tip
如果你对加密哈希是如何嵌入到IPFS的常规文件处理中的感兴趣，查看这个IPFS Camp 2019的视频！[Core Course: How IPFS Deals With Files](https://www.youtube.com/watch?v=Z5zNPwMDYGg)
:::

加密哈希是一个函数，它接受任意的输入信息，然后返回一个定长的值。这个特定值取决于给定的hash算法，如[SHA-1](https://en.wikipedia.org/wiki/SHA-1) (used by git), [SHA-256](https://en.wikipedia.org/wiki/SHA-2), 或者[BLAKE2](<https://en.wikipedia.org/wiki/BLAKE_(hash_function)#BLAKE2>)，但是一个给定的hash算法总会对给定的输入返回相同的值。阅读维基的[hash函数完整列表](https://en.wikipedia.org/wiki/List_of_hash_functions)以获取更多信息。

作为一个示例，以下输入：

```
Hello world
```

由**SHA-1**会表示为：

```
0x7B502C3A1F48C8609AE212CDFB639DEE39673F5E
```

然而相同的输入，由**SHA-256**则会表示为下面的输出：

```
0x64EC88CA00B268E5BA1A35678A1B5316D212F4F366B2477232534A8AECA37F3C
```

注意第二个hash要比第一个长。这是因为SHA-1创建的是160-bit的hash，而sha-256创建的是256-bit的hash。前置的`0x`说明后续的hash值是以十六进制来表示的。

hash可以以不同进制来表示（`base2`, `base16`, `base32`等）。实际上IPFS使用这个作为[内容标识符](/concepts/content-addressing/)的一部分，并且通过[Multibase](https://github.com/multiformats/multibase)协议，同时支持多种进制的表示。

例如，"Hello world"的SHA-256 hash值，可以按32进制表示为：

```
mtwirsqawjuoloq2gvtyug2tc3jbf5htm2zeo4rsknfiv3fdp46a
```

## Hash很重要

加密hash带来了一系列非常重要的特性：

- **确定性** - 相同的输入消息总是返回完全相同的输出hash值
- **非相关性** - 消息的一个很小变动，会生成完全不一样的hash值
- **唯一性** - 从两个不同的消息生成相同的hash值是不可行的
- **单向性** - 从其hash值来推测或者计算原始输入内容是不行的

这些特性也就意味这我们可以使用加密hash来标识任意的数据：hash对我们计算的数据来说是唯一的，而且它不会太长，因此通过网络发送它不会消耗太多的资源。hash是固定长度的，因此即便是1G的视频文件，其hash值也只有32字节。

这对于类似于IPFS的分布式系统来说至关重要，因为我们希望能够从很多地方来存储和检索数据。一个运行IPFS的计算机可以向所有它连接上的节点发出请求，查询它们是否有一个特定hash的文件，如果有的话，他们就会把整个文件发送回来。没有一个如同加密hash一样简短而唯一的标识符的话，这是不可能实现的。这个技术称为[内容寻址](/concepts/content-addressing/)，因为其内容本身被用来形成它的地址，而不是关于它所存储在的计算机及磁盘位置的信息。

## 内容标识符不是文件hash

hash函数被广泛用于检查文件的完整性。一个下载提供者可以发布其文件的hash函数输出值，通常称为校验和。校验和让用户可以验证文件自发布以来并没有被修改过，这是通过对生成该校验和的下载文件进行同样的hash函数计算，如果用户从下载文件计算的校验和与网站上提供的校验和一致，用户就可以确信文件没有被修改。

我们来看一个具体的例子。当你从[Ubuntu Linux](https://ubuntu.com/)上下载一个镜像时，你会看到Ubuntu网站上列出的以下`SHA-256`校验和，以供校验用：

```
0xB45165ED3CD437B9FFAD02A2AAD22A4DDC69162470E2622982889CE5826F6E3D ubuntu-20.04.1-desktop-amd64.iso
```

在下载Ubutnu镜像后，你可以通过计算该文件的hash并确保与网站的值一致，来验证其完整性：


```shell
echo "b45165ed3cd437b9ffad02a2aad22a4ddc69162470e2622982889ce5826f6e3d *ubuntu-20.04.1-desktop-amd64.iso" | shasum -a 256 --check

ubuntu-20.04.1-desktop-amd64.iso: OK
```

如果我们将`ubuntu-20.04.1-desktop-amd64.iso`文件添加到IPFS中，我们也会获得一个输出的hash值：

```shell
ipfs add ubuntu-20.04.1-desktop-amd64.iso

added QmPK1s3pNYLi9ERiq3BDxKa4XosgWwFRQUydHUtz4YgpqB ubuntu-20.04.1-desktop-amd64.iso
 2.59 GiB / 2.59 GiB [==========================================================================================] 100.00%
```

这个通过`ipfs add`命令返回的字符串`QmPK1s3pNYLi9ERiq3BDxKa4XosgWwFRQUydHUtz4YgpqB`，正是`ubuntu-20.04.1-desktop-amd64.iso`文件的内容标识符（CID）。我们可以使用[CID检测器](https://cid.ipfs.io/)来查看CID中包含的内容。其实际的hash值在`DIGEST (HEX)`后面：

```
NAME: sha2-256
BITS: 256
DIGEST (HEX): 0E7071C59DF3B9454D1D18A15270AA36D54F89606A576DC621757AFD44AD1D2E
```

::: tip
hash函数的名称使用并不一致。`SHA-2`, `SHA-256` 或`SHA-256 bit`都是指同一个hash函数。
:::

我们现在可以检查CID中包含的hash是否和文件的校验和一致了：

```shell
echo "0E7071C59DF3B9454D1D18A15270AA36D54F89606A576DC621757AFD44AD1D2E *ubuntu-20.04.1-desktop-amd64.iso" | shasum -a 256 --check

ubuntu-20.04.1-desktop-amd64.iso: FAILED
shasum: WARNING: 1 computed checksum did NOT match
```

正如我们所见的，CID中的hash值和输入文件的hash值并不一致。为了了解CID中包含的hash值到底是什么，我们必须了解IPFS是如何存储文件的。IPFS使用了[有向无环图(DAG)](/concepts/merkle-dag/)来保持对IPFS中所存储数据的跟踪。一个CID就标识着这个图中一个特定节点。这个标识就是对这个节点内容的加密hash的结果值（如`SHA256`）。
