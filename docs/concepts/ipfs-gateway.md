---
title: IPFS网关
description: Learn why gateways are an important part of using IPFS in conjunction with the legacy web.
related:
  'IPFS Docs: Address IPFS on the Web': /how-to/address-ipfs-on-web/
  'IPFS public gateway checker': https://ipfs.github.io/public-gateway-checker/
  'GitHub repo: Gateway summary from go-ipfs': https://github.com/ipfs/go-ipfs/blob/master/docs/gateway.md
  'Article: Solving the IPFS Gateway Problem (Pinata)': https://medium.com/pinata/the-ipfs-gateway-problem-64bbe7eb8170
  'Tutorial: Setting up an IPFS gateway on Google Cloud Platform (Stacktical)': https://blog.stacktical.com/ipfs/gateway/dapp/2019/09/21/ipfs-server-google-cloud-platform.html
---

# IPFS网关

本文档讨论了：

- 几种网关的类型。
- 网关在IPFS中的角色。
- 网关的适用场景。
- 不应使用网关的场景。
- 实施指南。

你有如下需求时，应该阅读此文档：

- 在概念层面理解网关是如何与IPFS的整体使用相适配的。
- 确定在你的应用场景中是否应该使用，以及该使用何种类型的网关。
- 从概念层面理解如何在你的应用场景中部署网关。

## 概览

IPFS部署旨在使得所有流行的浏览器和工具中原生支持IPFS，而网关则为那些还没有原生支持IPFS的应用提供解决方案。例如，当一个还没有支持IPFS的浏览器尝试以`ipfs://{CID}/{optional path to resource}`的规范形式来访问IPFS内容时，会发生错误。其它仅仅依赖HTTP的工具，在访问IPFS规范形式的内容时也会遇到类似的错误，如[Curl](https://curl.haxx.se/) 和[Wget](https://www.gnu.org/software/wget/)。


像[IPFS伴侣](https://github.com/ipfs-shipyard/ipfs-companion)之类的工具解决了这些内容访问的错误问题。然而，不是每个用户都有权或者有能力去修改他们电脑的配置。IPFS则提供了一个基于HTTP的服务，允许不理解IPFS的浏览器和工具也能够访问IPFS内容。

## 网关提供方

无论是谁在何处部署的IPFS网关，它都能够处理对IPFS[内容标识符](/concepts/content-addressing)的访问请求。因此为了获取最佳性能，在需要网关服务时，应该使用离你最近的网关。

### 本地网关

你的主机可以提供一个本地网关服务，如`localhost:8080`。如果你已经安装了[IPFS Desktop](https://github.com/ipfs-shipyard/ipfs-desktop#ipfs-desktop)或者其他形式的IPFS节点，就已经有一个本地网关服务了。

### 专有网关

运行[IPFS Desktop](https://github.com/ipfs-shipyard/ipfs-desktop#ipfs-desktop)或其他形式的IPFS节点会触发到其他IPFS节点的连接。专有网络的管理员可能会将此类连接看作潜在的安全漏洞。在专有网络中的专有IPFS网关服务器可以运行一个可信代码库以提供替代架构，用于提供对外部托管的IPFS内容的读/写访问。

防火墙之后的网关仅仅代表了专有网关的一种潜在位置。更通用的说，只要将一个网关配置为对特定域名或者公开互联网的一部分限制访问时，就可以将其视为专有网关。这篇[在Google Cloud平台上配置IPFS网关的教程](https://blog.stacktical.com/ipfs/gateway/dapp/2019/09/21/ipfs-server-google-cloud-platform.html)就包括了对限制访问的描述。

### 公共网关

公共网关运营商包括：

- Protocol Labs，部署了公共网关：`https://ipfs.io`。
- 第三方公共网关，如：`https://cf-ipfs.com`。

Protocol Labs维护了一个[公共网关列表](https://ipfs.github.io/public-gateway-checker/)及其状态。

![A list of public gateways and their status, available on IPFS](./images/ipfs-gateways/public-gateway-checker.png)

## 网关类型

可以从这几个维度来将网关分类：

- [读/写支持](#read-only-and-writeable-gateways)
- [解析方式](#resolution-style)
- [服务](#gateway-services)

可以从安全性、性能和其他功能影响的角度来选择要使用的网关形式。

### 只读及可写网关

之前章节讨论的示例主要说明了只读的HTTP网关，用于通过HTTP GET方法从IPFS获取内容。可写的HTTP网关还支持`POST`, `PUT`, 和`DELETE`方法。

### 解析方式

存在三种解析方式：

- [路径](#path)
- [子域名](#subdomain)
- [DNSLink](#dnslink)

#### 路径

上面讨论的示例采用了路径解析：

```bash
https://{gateway URL}/ipfs/{content ID}/{optional path to resource}
```

但是，路径解析网关违背了[同源策略](https://en.wikipedia.org/wiki/Same-origin_policy)，该策略用于保护一个网站不会被其他网站不当的访问其会话数据。

#### 子域名

子域名解析方式保持了与[单一来源策略](https://en.wikipedia.org/wiki/Same-origin_policy)的合规性。其访问的规范形式，`https://{CID}.ipfs.{gatewayURL}/{optional path to resource}`，使得浏览器将每个返回的文件解析为来自不同的源。

子域名解析从[Go-IPFS](https://github.com/ipfs/go-ipfs)发行版`0.5.0`开始支持。

#### DNSlink

只要IPFS的数据内容发生了变化，IPFS都会基于其数据内容创建一个新的CID。许多应用程序都需要访问网站或者文件的最新版本，但是并不知道其最新版本的CID。[星际名字服务(IPNS)](/concepts/ipns)允许通过一个版本无关的IPNS标识符来表示IPFS最新版本内容的CID。

版本无关的IPNS标识符包含一个hash。当一个网关处理形如`https://{gatewayURL}/ipns/{IPNS identifier}/{optional path}`的请求时，网关使用IPNS来把IPNS标识符解析为当前版本的CID，然后再获取其对应的内容。

但是IPNS标识符也可能指向一个完整的域名，类似于`example.com`的通用形式。

当网关识别到IPNS标识符中包含类似`example.com`的域名形式时，就会触发DNSLink解析。例如，URL `https://libp2p.io`会返回存储在IPFS中的当前版本的网站信息，如下：

1. 网关收到一个如下形式的请求：

   ```bash
   https://{gateway URL}/ipns/{example.com}/{optional path}
   ```

1. 网关会向DNS查询请求中域名`{example.com}`的TXT 记录，其响应为一个字符串形式，类似于`dnslink=/ipfs/{CID}`或 `_dnslink=/ipfs/{CID}`。如果查询到了结果，网关就会使用对应的CID提供服务：`ipfs://{CID}/{optional path}`。和路径解析方式一样，DNSLink解析形式也违背了单源策略。域名服务商可以通过在DNS中添加一个`Alias`别名记录来指向合适的IPFS网关，从而确保了单源策略的合规性，以及当前版本内容的交付。如：`gateway.ipfs.io`。

1. `Alias`别名记录会将到`example.com`的任何访问重定向到指定的网关上。因此从浏览器中访问`https://{example.com}/{optional path to resource}`，会重定向到`Alias`别名所指定的网关地址上。

1. 网关采用DNSLink解析来返回IPFS中的当前内容版本。

1. 浏览器不会将网关视为内容的来源，因此会在`example.com`上强制执行单源策略。

### 网关服务

当前HTTP网关可以同时访问IPFS和IPNS服务：

| 服务 | 形式     | 访问规范形式                                                                                                                                                                      |
| ------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IPFS    | 路径      | `https://{gateway URL}/ipfs/{CID}/{optional path to resource}`                                                                                                                                |
| IPFS    | 子网关 | `https://{CID}.ipfs.{gatewayURL}/{optional path to resource}`                                                                                                                                 |
| IPFS    | DNSLink   | `https://{example.com}/{optional path to resource}` **推荐**, 或者 <br>`https://{gateway URL}/ipns/{example.com}/{optional path to resource}`                                              |
| IPNS    | 路径      | `https://{gateway URL}/ipns/{IPNS identifier}/{optional path to resource}`                                                                                                                    |
| IPNS    | 子网关 | `https://{IPNS identifier}.ipns.{gatewayURL}/{optional path to resource}`                                                                                                                     |
| IPNS    | DNSLink   | 当IPNS标识符是一个域名时有用： <br>`https://{example.com}/{optional path to resource}` **推荐**, 或者 <br>`https://{gateway URL}/ipns/{example.com}/{optional path to resource}` |

### 使用哪种类型

首选的网关访问形式应该取决于目标内容的特性。

| 目标                                          | 首选网关类型 | 访问规范格式 <br> 功能与注意事项                                                                                                                                                                                                                                                                     |
| ----------------------------------------------- | ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 潜在可变的根内容 | IPNS子网关         | `https://{IPNS identifier}.ipns.{gatewayURL}/{optional path to resource}` <br> + 支持跨域安全性 <br> + 支持跨域资源共享 <br> + 同时适用于IPNS域名名称(`{domain.tld}`)和IPNS hash名称                                                                               |
|                                                 | IPFS DNSLink           | `https://{example.com}/{optional path to resource}` <br> + 支持跨域安全性 <br> + 支持跨域资源共享 <br> – 需要更新DNS以将根内容的修改传播出去 <br> • 由DNSLink而非用户/应用指定了要使用的网关，可能会带来网关信任和拥塞问题 |
| 不可变的根或内容                  | IPFS子域名         | `https://{CID}.ipfs.{gatewayURL}/{optional path to resource}` <br> + 支持跨域安全性<br> + 支持跨域资源共享                                                                                                                                                                           |

任何形式的网关都为没有原生支持IPFS的应用提供了一个桥梁。同时应用中原生的IPFS实现会带来更好的性能和安全性。

## 不应使用网关的场景

### 对延迟敏感的应用程序

任何网关都会在完成所需操作的同时带来延迟，因为网关充当了请求源头和能够提供所需内容的IPFS节点之间的中介。如果提供服务的网关提前缓存了需要的内容（如由于之前的请求），那么缓存能够缓解这个延迟。

网关的负载过高也会导致延迟，因为请求需要排队等待。

在进行延迟敏感的操作时，应该尽量在应用中使用内置IPFS节点（最快），或者访问本地守护服务（几乎一样快）。如果都不行的话，可以使用一个作为本地服务安装的网关。注意当在本地运行IPFS节点时，它提供了一个在`http://127.0.0.1:8080`上的网关。

所有对耗时不敏感的处理都可以通过公共/专有网关来请求路由。

### 需要端到端的加密验证

出于对第三方网关漏洞的担心，需要对内容读写进行端到端验证的应用应该尽可能避免使用网关。如果必须使用外部网关，此类应用可以使用`ipfs.io`或其他可信的第三方网关。

## 限制与潜在解决办法

### 中心化

网关的使用导致了对基于位置寻址的需求：`https://{gatewayURL}/ipfs/{CID}/{etc}`，网关URL也容易会被用户作为内容标识的句柄；即统一资源定位（URL）（不正确的）等同于了统一资源标识符（URI）。设想网关离线或者无法被其他位于防火墙之后的用户所访问的场景，此时不正确的被网关URL所标识的内容也表现为不可访问，从而破坏了IPFS的一个关键优势：去中心化。

类似的，DNSLink解析与`Alias`别名一起的使用，因为在DNS TXT记录中指定的`dnslink={value}`，也强制请求要通过指定的网关。如果指定的网关负载过高、离线或者收到威胁，所有其中的流量也都会被删除、禁用或者不可信了。

### 信任错位

信任一个特定的网关，也就是说，需要你去信任网关的证书颁发机构，以及网关使用的公钥基础设施的安全性。如果证书颁发机构或者公钥基础设施遭到破坏，同样会破坏网关的可信度。

### 违背同源策略

为了避免网站不当的访问了其他网站的HTTP会话数据，[同源策略](https://en.wikipedia.org/wiki/Same-origin_policy)只允许脚本去访问拥有相同域名和端口的页面。

考虑存在IPFS中的两个网页：`ipfs://{CID A}/{webpage A}`和`ipfs://{CID B}/{webpage B}`，在`webpage A`上的代码应该不允许访问`webpage B`中的数据，因为他们不具备相同的内容ID（不同源）。

然而，当浏览器通过一个网关来访问这两个网站的时候，可能会无法保障这个安全策略。从浏览器的角度来看，两个网页都有相同的源：URL `https://{gatewayURL}/...`中的网关地址。

使用子域名网关则避免了违背同源策略。在这个情形下，网关对这两个页面的引用分别是：

```bash
https://{CID A}.ipfs.{gatewayURL}/{webpage A}
https://{CID B}.ipfs.{gatewayURL}/{webpage B}
```

这些页面并没有使用相同的源。同样DNSLink网关的使用也可以避免违背同源策略。[IPFS公共网关检查器](https://ipfs.github.io/public-gateway-checker/)可以识别避免了违背同源策略的网关。

### 跨域资源共享 (CORS)

[CORS](https://web.archive.org/web/20200418003728/https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#The_HTTP_response_headers)允许网页被授权访问不同源的特定数据。[IPFS公共网关检查器](https://ipfs.github.io/public-gateway-checker/)可以识别支持CORS的网关。

### 网关中间人漏洞

使用公共或专有HTTP网关牺牲了对正确内容传输的端到端加密验证。考虑这个场景，浏览器从`https://ExampleGateway.com/ipfs/{cid}`获取内容，一个遭破坏的`ExampleGateway.com`可能存在中间人漏洞，包括：

- 用虚假内容替代通过CID检索到的实际内容。
- 将请求和响应内容的副本，包括请求方浏览器的IP地址信息一起发送给第三方。

一个遭破坏的可写网关可能会将伪造的内容注入到IPFS网络中，从而返回一个用户认为指向正确结果的CID。例如：

1. Alice提交了余额`123.54`到一个遭破坏的可写网关。
1. 网关实际将余额存为`0.00`，并返回这个伪造内容的CID给Alice。
1. Alice将这个伪造内容的CID发送给Bob。
1. Bob通过CID取得内容，并加密验证了余额值为`0.00`。

为了部分的解决这个问题，可以使用这个公共网关[cf-ipfs.com](https://cf-ipfs.com)来作为一个独立可信的参考，它也支持同源策略和CORS。

### 下载文件时的假定文件名

下载文件时，浏览器通常会通过路径的最后一部分来推测其文件名，如`https://{domainName/tld}/{path}/userManual.pdf`会下载并在本地存储一个名为`userManual.pdf`的文件。但是如果在IPFS中直接链接到一个不包含目录的文件时，CID就是其路径的最后组成部分，而下载并保存为CID的文件名无法通过人性化设计的测试。

为了解决这个问题，可以在查询的请求串之后添加`?filename={filename.ext}`参数，以预先指定要下载文件的本地存储文件名：

| 形式     | 查询语句                                                                                                                                                     |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 路径      | `https://{gatewayURL}/ipfs/{CID}/{optional path to resource}?filename={filename.ext}`                                                                     |
| 子域名 | `https://{CID}.ipfs.{gatewayURL}/{optional path to resource}?filename={filename.ext}`                                                                     |
| DNSLink   | `https://{example.com}/{optional path to resource}` 或 <br> `https://{gatewayURL}/ipns/{example.com}/{optional path to resource}?filename={filename.ext}` |

### 陈旧的缓存

网关默认会为来自DNS TXT记录的DNSLink信息提供一个小时的缓存生命期。内容更改后，缓存的DNSLink仍然会继续指向过期的CID。为了限制过期缓存内容的传输，域名服务商应该将DNS记录的time-to-live生存期参数设为`60`即一分钟。

## 常见问题(FAQs)

### 什么是ipfs.io网关？

ipfs.io网关使得互联网用户可以访问和查看IPFS网络中第三方托管的数据。ipfs.io网关是一个由Protocol Labs运行的社区资源，用于帮助开发者在IPFS中构建应用。

### ipfs.io网关与其他网关有何不同？

ipfs.io网关是由Protocol Labs运行的网关。许多其他实体也会运行他们自己的网关，并基于其本地法律和法规的约束，在限制和访问方面提供不同的策略。这里有一个[可用的公共网关列表](https://ipfs.github.io/public-gateway-checker/).

Protocol Labs不会存储或者托管通过ipfs.io可查看的数据。相反，ipfs.io允许用户查看托管在第三方的内容。Protocol Labs对通过ipfs.io可查看的数据，以及其他网关也没有任何控制权。

### ipfs.io网关是否是一个数据存储主机？

不。ipfs.io网关是一个用于访问IPFS网络中托管在第三方节点数据的通行门户。它不是一个数据存储主机。

### 网站是否可以依托于ipfs.io网关来托管？

不。网站不能以任何形式依托于ipfs.io网关来托管。ipfs.io网关是Protocol Labs运行的一个社区资源，用于帮助开发者在IPFS中构建应用。ipfs.io网关的用户应该谨慎的使用其资源。Protocol Labs会限制或者禁用那些过度使用或者滥用社区资源的用户，包括依托于ipfs.io网关来进行网站托管或者违背了社区行为准则。

### ipfs.io网关是如何应对全球的数据法规的？

Protocol Labs遵守相关司法管辖区的法律法规。

如上所述，ipfs.io网关不是网站托管服务商或者数据存储服务商，且Protocol Labs无法删除任何通过ipfs.io网关可访问的内容。

### 谁对通过ipfs.io网关可查看的内容负责？

ipfs.io网关的用户在使用ipfs.io网关网关时，必须遵守所有其适用的法律法规。

ipfs.io网关不是一个数据存储服务商或者网站托管商。ipfs.io网关允许用户查看托管于第三方的内容，但Protocol Labs没有其控制权。某些内容可以通过ipfs.io网关访问并不意味着它托管在ipfs.io网关中或者Protocol Labs可以做任何事情来删除该内容。

如上所述，ipfs.io网关不是一个网站托管服务商或者数据存储服务商，Protocol Labs也无法删除通过ipfs.io网关可访问到的互联网的内容。如果你确信通过ipfs.io网关所访问到的内容是非法的或者侵犯了你的版权，我们鼓励你直接通知托管或者控制该数据的任何人。

虽然ipfs.io网关并不作为数据或者网站托管商提供服务，在特定情形下，Protocol Labs可以禁用通过ipfs.io网关对某些特定内容的访问。这并不意味着数据已经从网络中删除了，而仅仅是通过ipfs.io网关无法访问了。这也不会影响该数据通过其他团体网关来访问的可用性。

可以通过发送邮件到abuse@protocol.ai来举报滥用行为。适当情形下，我们会根据你的举报内容来禁用某些特定内容通过ipfs.io网关的可访问性。

### Protocol Labs是否可以删除可以通过ipfs.io gateway访问的内容？

不。ipfs.io网关只是互联网上第三方存储的内容的许多可访问入口之一。Protocol Labs没有托管这些内容，也无法删除它，但是在特定情形下，它可以禁止用户通过ipfs.io网关来访问该内容。

## 了解更多

- [Gateway configuration options](https://github.com/ipfs/go-ipfs/blob/master/docs/config.md#gateway)
