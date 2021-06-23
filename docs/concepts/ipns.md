---
title: IPNS
legacyUrl: https://docs.ipfs.io/guides/concepts/ipns/
description: Learn about the InterPlanetary Name System (IPNS) and how it can be used in conjunction with IPFS.
---

# 星际名字系统(IPNS)

IPFS使用[基于内容的寻址](/concepts/content-addressing/)；它基于文件内的内容创建了文件的地址。如果你曾经将一个类似于`/ipfs/QmbezGequPwcsWo8UL4wDF6a8hYwM1hmbzYv2mnKkEWaUp`的IPFS地址分享给某人，每次你更新了文件内容时，都需要提供一个新的链接地址。

星际名字系统(IPNS)通过创建一个可更新的地址，解决了这个问题。

IPNS中的 _名字_ 是一个公钥的[hash](/concepts/hashing)。它关联了一条记录，该记录包含了它所链接信息的hash值，并使用对应的私钥进行签名。任何时候都可以签名并发布新的记录。 It is associated with a record containing information about the hash it links to that is signed by the corresponding private key. New records can be signed and published at any time.

查找IPNS地址的时候，使用`/ipns/`前缀：

```bash
/ipns/QmSrPmbaUKA3ZodhzPWZnpFgcPMFWF4QsxXbkWfEptTBJd
```

## 使用CLI设置IPNS的示例

1. 如果还没有启动IPFS守护进程，首先启动它：

   ```bash
   ipfs daemon
   ```

1. 创建一个想通过IPNS来设置的文件。本教程中，我们会创建一个简单的 _hello world_ 文件：

   ```bash
   echo "Hello, world!" > hello.txt
   ```

1. 将文件添加到IPFS中：

   ```bash
   ipfs add hello.txt

   > added QmaMLRsvmDRCezZe2iebcKWtEzKNjBaQfwcu7mcpdm8eY2 hello.txt
   > 13 B / 13 B [===================================================================] 100.00%
   ```

   记住IPFS输出的`Qm` hash值。

1. 通过`cat`刚才获得的`Qm` hash值，重新查看文件内容：

   ```bash
   ipfs cat QmaMLRsvmDRCezZe2iebcKWtEzKNjBaQfwcu7mcpdm8eY2

   > Hello, world!
   ```

1. 将此`Qm` hash发布到IPNS中：

   ```bash
   ipfs name publish /ipfs/QmaMLRsvmDRCezZe2iebcKWtEzKNjBaQfwcu7mcpdm8eY2

   > Published to k51qzi5uqu5dkkciu33khkzbcmxtyhn376i1e83tya8kuy7z9euedzyr5nhoew: /ipfs/QmaMLRsvmDRCezZe2iebcKWtEzKNjBaQfwcu7mcpdm8eY2
   ```

   `k51...`就是你安装的IPFS节点的密钥。

1. 现在可以通过打开`https://gateway.ipfs.io/ipns/k51qzi5uqu5dkkciu33khkzbcmxtyhn376i1e83tya8kuy7z9euedzyr5nhoew`来查看你的文件：

   ```bash
   curl https://gateway.ipfs.io/ipns/k51qzi5uqu5dkkciu33khkzbcmxtyhn376i1e83tya8kuy7z9euedzyr5nhoew

   > Hello, world!
   ```

1. 修改文件，再添加到IPFS中，并更新IPNS：

   ```bash
   echo "Hello IPFS!" > hello.txt
   ipfs add hello.txt

   > added QmUVTKsrYJpaxUT7dr9FpKq6AoKHhEM7eG1ZHGL56haKLG hello.txt
   > 11 B / 11 B [=============================================================================================] 100.00%

   ipfs name publish QmUVTKsrYJpaxUT7dr9FpKq6AoKHhEM7eG1ZHGL56haKLG

   > Published to k51qzi5uqu5dkkciu33khkzbcmxtyhn376i1e83tya8kuy7z9euedzyr5nhoew: /ipfs/QmUVTKsrYJpaxUT7dr9FpKq6AoKHhEM7eG1ZHGL56haKLG
   ```

1. 现在可以重新打开`https://gateway.ipfs.io/ipns/k51qzi5uqu5dkkciu33khkzbcmxtyhn376i1e83tya8kuy7z9euedzyr5nhoew`，以查看相同地址下已更新的文件：

   ```bash
   curl https://gateway.ipfs.io/ipns/k51qzi5uqu5dkkciu33khkzbcmxtyhn376i1e83tya8kuy7z9euedzyr5nhoew

   > Hello IPFS!
   ```

可以通过`name resolve`命令查看与你的`k5`密钥所关联文件的 `Qm` hash值：

```bash
ipfs name resolve

> /ipfs/QmUVTKsrYJpaxUT7dr9FpKq6AoKHhEM7eG1ZHGL56haKLG
```

要使用一个不同的`k5`密钥，首先使用`key gen test`来创建一个，然后在`name publish`时使用`--key`标志：

```bash
ipfs key gen SecondKey

> k51qzi5uqu5dh5kbbff1ucw3ksphpy3vxx4en4dbtfh90pvw4mzd8nfm5r5fnl

ipfs name publish --key=SecondKey /ipfs/QmUVTKsrYJpaxUT7dr9FpKq6AoKHhEM7eG1ZHGL56haKLG

> Published to k51qzi5uqu5dh5kbbff1ucw3ksphpy3vxx4en4dbtfh90pvw4mzd8nfm5r5fnl: /ipfs/QmUVTKsrYJpaxUT7dr9FpKq6AoKHhEM7eG1ZHGL56haKLG
```

## JS SDK API配置IPNS示例

假设你准备把你的网站发不到IPFS上。你可以使用[文件API](/concepts/file-systems/#mutable-file-system-mfs)来发布你的静态网站，然后将获得一个链接到该网站的CID。但当你需要进行修改的时候，出现了一个问题：你将获得一个新的CID，因为现在有着不同的内容。每次都给别人一个新的地址也是不现实的。

这正是名字API适用的场合。使用它可以创建一个单一的，稳定的IPNS地址，它总是指向网站最新版本内容的CID。

```js
// The address of your files.
const addr = '/ipfs/QmbezGequPwcsWo8UL4wDF6a8hYwM1hmbzYv2mnKkEWaUp'

ipfs.name.publish(addr).then(function (res) {
  // You now receive a res which contains two fields:
  //   - name: the name under which the content was published.
  //   - value: the "real" address to which Name points.
  console.log(`https://gateway.ipfs.io/ipns/${res.name}`)
})
```

以同样的方式，可以在同一个地址下，重新发布网站的新版本。默认情况下，`ipfs.name.publish`使用的是节点ID为密钥。

## IPNS的替选

IPNS不是IPFS中创建可变地址的唯一方式。你也可以使用[DNSLink](/concepts/dnslink/)，它当前比IPNS更快，而且使用了人类可读的名字。还有其他社区成员正在探索使用区块链来存储通用名字记录。
