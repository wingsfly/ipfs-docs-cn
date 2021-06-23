---
title: 内容寻址
legacyUrl: https://docs.ipfs.io/guides/concepts/cid/
description: Learn about how content addressing works and how content identifiers, or CIDs, play a crucial role in IPFS.
---

# 内容寻址与CID

::: callout
要深入探讨内容标识符（CID）是如何构建的，可以查看ProtoSchool的教程：[Anatomy of a CID](https://proto.school/anatomy-of-a-cid)。
:::

[[toc]]

内容标识符，或称为CID，是用来指向IPFS中信息的标签。它没有指定内容存储的位置，而是基于内容本身形成了一类地址。CID相对它代表的内容来说是很简短的。

CID是基于内容的[加密hash](/concepts/hashing/)生成的，这意味着：

- 内容中的任何一处不同都会产生一个不同的CID，同时
- 添加到不同IPFS节点（相同设置）上的同一份内容，将会生成相同的CID。

IPFS默认使用`sha-256`哈希算法，也支持许多其他的算法。[Multihash](https://multiformats.io/multihash/)项目代表了这方面的进展，其目的是使应用程序对哈希的使用具备前瞻性，因此允许多种hash函数同时存在。（如果你想知道IPFS是如何决定hash的类型的，可以持续关注[这个论坛讨论](https://discuss.ipfs.io/t/who-decides-what-hashing-algorithms-ipfs-allows/6742)）。

## 标识符格式

因为基础编码格式或CID版本的不同，CID可以有几种不同的格式。许多现存的IPFS工具还在生成v0版本的CID，尽管`files` ([Mutable File System](/concepts/file-systems/#mutable-file-system-mfs))和`object`操作默认都已经使用了CIDv1版本。

### Version 0 (v0)

刚开始设计IPFS时，使用的是base58编码的多重哈希作为文件标识符。这个比新的CID要简单，但灵活性差了很多。CIDv0仍然有需有IPFS操作在默认使用，所以通常还需要支持v0协议。

如果一个CID是46字符长度，且以“Qm”开头，它就是CIDv0格式的（更多细节参看CID规范中的[解码算法](https://github.com/ipld/cid/blob/ef1b2002394b15b1e6c26c30545fd485f2c4c138/README.md#decoding-algorithm)）。

### Version 1 (v1)

CID v1包含一些前导标识符以明确说明使用的表达方式，以及内容hash本身，其中包括：

- 一个[multibase](https://github.com/multiformats/multibase)前缀，指定CID用于其他部分的编码
- 一个CID版本标识符，指示这是哪个版本的CID
- 一个[multicodec](https://github.com/multiformats/multicodec)标识符，表示目标内容的格式，这个可以用于帮助人们和软件程序知悉在获取内容后该如何解析内容。

这些前导标识符同时也提供了向前兼容性，以支持未来CID版本使用的不同格式。

可以通过CID前面若干字节来解析内容地址的其余部分，并知道该如何在从IPFS获得内容后对其进行解码。更多细节可以查看[CID规范](https://github.com/ipld/cid)。它包含了[解码算法](https://github.com/ipld/cid/blob/ef1b2002394b15b1e6c26c30545fd485f2c4c138/README.md#decoding-algorithm)和解码CID的已有软件实现的链接。

如果在CIDv0和CIDv1中无法选择，那就为你的新项目选择CIDv1，可以通过传入一个版本标志（`ipfs add --cid-version 1`）来选择。这样更具前瞻性，且在[浏览器上下文中使用更加安全](/how-to/address-ipfs-on-web/#subdomain-gateway)。

IPFS项目在最近的更新中会切换到CIDv1作为新的默认值。

## CID检查器

自行探索CID很简单，想要拆分某个CID的multibase, multicodec, 或multihash信息的话，可以使用[CID检查器](https://cid.ipfs.io/#QmY7Yh4UquoXHLPFo2XbhXkhBvFoPwmQUSa92pxnxjQuPU)或者[IPLD浏览器中的CID信息面板](https://explore.ipld.io/#/explore/QmY7Yh4UquoXHLPFo2XbhXkhBvFoPwmQUSa92pxnxjQuPU) (以上两个链接使用的是同一个示例CID)来交互式拆分CID。

查看ProtoSchool的[CID剖析](https://proto.school/anatomy-of-a-cid)教程，以了解一个文件是如何在多个CID版本中表示的。

## CID转换

可以将CID从v0转换为v1，使它能够以multibase编码来表示。
CIDv1的默认格式是不区分大小写的`base32`，但鼓励针对IPNS名字使用更短的`base36`，以确保和[子域名](/how-to/address-ipfs-on-web/#subdomain-gateway)的文本表示方式一致。

### v0转换为v1

可以在命令行中运行自带的`ipfs cid format`命令：

```
$ ipfs cid format -v 1 -b base32 QmbWqxBEKC3P8tqsKc98xmWNzrzDtRLMiMPL8wBuTGsMnR
bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi
```

JavaScript用户还可以利用[`cids`](https://www.npmjs.com/package/cids)库提供的`toV1()`方法：
```js
const CID = require('cids')
new CID('QmbWqxBEKC3P8tqsKc98xmWNzrzDtRLMiMPL8wBuTGsMnR').toV1().toString()
// → bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi
```

### 文本转换为二进制

CID可以同时表示为文本，以及一个字节流。后者在考虑速度和存储效率时可以是更好的选择。

要把一个CIDv1从文本转换为二进制格式，只要简单的读取第一个字符，然后用[multibase table](https://github.com/multiformats/multibase#multibase-table)规范中定义的编码规范来解码剩余的字符。

JS用户可以使用[`cids`](https://www.npmjs.com/package/cids)库来获得一个二进制版本格式，其类型为`Uint8Array`：

```js
const CID = require('cids')
new CID('bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi').bytes
// → Uint8Array [ 1, 112,  18,  32, 195, 196, 115,  62, ... ]
```

::: warning 注意要正确解析CID，不要走捷径。

除非你是把数据导入IPFS的人，否则CID的长度是不确定的，是由其内部的multihash长度所决定的。

为了说明这一点，传递自定义的哈希函数会产生不同长度的哈希值：

```
$ ipfs add --cid-version 1 --hash sha2-256    -nq cat.jpg | wc -c
60
$ ipfs add --cid-version 1 --hash blake2b-256 -nq cat.jpg | wc -c
63
$ ipfs add --cid-version 1 --hash sha3-512    -nq cat.jpg | wc -c
111
```
:::


### CID转换为hex

有时候出于调试目的，会需要原始字节的[十六进制](https://en.wikipedia.org/wiki/Hexadecimal)表示。
为了获取一个完整CID的原始`.bytes`字节流的十六进制表示，可以使用自带的`base16`编码并忽略其中的`f` (multibase前缀)：

```javascript
> cid.toString('base16')
'f01701220c3c4733ec8affd06cf9e9ff50ffc6bcd2ec85a6170004bb709669c31de94391a'

> cid.toString('base16').substring(1)
'01701220c3c4733ec8affd06cf9e9ff50ffc6bcd2ec85a6170004bb709669c31de94391a' // "cid as hex"
```

要转换回CIDv1格式，在十六进制值前面加上`f`（小写的十六进制编码[multibase前缀](https://github.com/multiformats/multibase#multibase-table)）。
可以按原样使用它（这是一个[有效的CID](https://ipfs.io/ipfs/f01701220c3c4733ec8affd06cf9e9ff50ffc6bcd2ec85a6170004bb709669c31de94391a)），或者将其作为参数传给`toString`，以转换为不同的multibase：

```javascript
> new CID('f' +'01701220c3c4733ec8affd06cf9e9ff50ffc6bcd2ec85a6170004bb709669c31de94391a').toString('base32')
// → bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi
```

::: tip
[子域名网关](/how-to/address-ipfs-on-web/#subdomain-gateway)将自定义路径格式如base16转换为base32或base36，以适应在DNS标签中的CID格式：
- [dweb.link/ipfs/f01701220c3c4733ec8affd06cf9e9ff50ffc6bcd2ec85a6170004bb709669c31de94391a](https://dweb.link/ipfs/f01701220c3c4733ec8affd06cf9e9ff50ffc6bcd2ec85a6170004bb709669c31de94391a)
  返回HTTP 301重定向：
  → [bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi.ipfs.dweb.link](https://bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi.ipfs.dweb.link/)
:::


## 更多资源

查看以下链接以获取关于CID及其如何运作的工作信息：

- [Core Course: How IPFS Deals With Files](https://www.youtube.com/watch?v=Z5zNPwMDYGg)
- [Files and IPFS Companion](https://www.youtube.com/watch?v=OCv5PvLnk-Y)
