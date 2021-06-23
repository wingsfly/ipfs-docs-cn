---
title: DNSLink
legacyUrl: https://docs.ipfs.io/guides/concepts/dnslink/
description: Learn how to map IPFS content to DNS names using DNSLink.
---

# DNSLink

DNSLink使用[DNS `TXT`记录](https://en.wikipedia.org/wiki/TXT_record) 将一个DNS名字，如`ipfs.io`映射为一个IPFS地址。因为你能够编辑自己的DNS记录，因此可以使用它来一直指向IPFS中对象的最新版本。因为DNSLink使用的是DNS记录，可以为其指定易于输入、阅读和记住的名称、路径和子域名。

DNSLink地址看起来类似于[IPNS](/guides/concepts/ipns)地址，但是使用了一个域名，而不是一个公钥hash：

```
/ipns/example.org
```

和普通的IPFS地址一样，它们也可以链接到其他文件，或者其他IPFS所支持的资源，像目录或者符号链接：

```
/ipns/example.org/media/
```

## 发布内容路径

使用带有`_dnslink`前缀的主机名来发布一个DNS `TXT`记录。

这不仅使DNSLink查询更加高效，因为只需要返回相关的`TXT`记录；而且提升了自动配置或者委托第三方来控制DNSLink记录的安全性，因为无需将原始DNS域的完全控制权提供出去。

例如，[`docs.ipfs.io`](https://docs.ipfs.io)页面可以被正确访问，因为存在一个`_dnslink.docs.ipfs.io`的`TXT`记录。如果查询`_dnslink.docs.ipfs.io`的DNS记录，就可以看到对应的DNSLink入口：

```shell
dig +noall +answer TXT \_dnslink.docs.ipfs.io
> \_dnslink.docs.ipfs.io. 30 IN TXT "dnslink=/ipfs/bafybeieenxnjdjm7vbr5zdwemaun4sw4iy7h4imlvvl433q6gzjg6awdpq"

```

## 解析DNSLink名字

当一个IPFS客户端或节点尝试解析地址时，它会查询一个前缀为`dnslink=`的`TXT`记录。其剩余部分可能是一个`/ipfs/`链接（如下面的示例），或者`/ipns/`，甚至是一个到其他DNSLink的链接。

```

dnslink=/ipfs/<CID for your content here>

```

例如，我们可以回到之前查询`_dnslink.docs.ipfs.io`DNS记录的地方，然后查看其DNSLink入口：

```sh
$ dig +noall +answer TXT _dnslink.docs.ipfs.io
_dnslink.docs.ipfs.io.  34  IN  TXT "dnslink=/ipfs/QmVMxjouRQCA2QykL5Rc77DvjfaX6m8NL6RyHXRTaZ9iya"
```

因此，这个地址：

```
/ipns/docs.ipfs.io/introduction/
```

会对应于这个块：

```
/ipfs/QmVMxjouRQCA2QykL5Rc77DvjfaX6m8NL6RyHXRTaZ9iya/introduction/
```

## 更多资源

需要完整的DNSLink指南 — 包括教程、用例和FAQ，查看[dnslink.io](http://dnslink.io/)。
