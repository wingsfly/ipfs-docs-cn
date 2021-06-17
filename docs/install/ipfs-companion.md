---
title: IPFS伴侣
description: The IPFS Companion browser extension allows you to interact with your IPFS node and the extended IPFS network through your browser. Learn how to install it here.
---

# IPFS伴侣

IPFS伴侣支持通过浏览器来与IPFS本地节点和外部网络进行交互。该插件在Brave, Chrome, Edge, Firefox, and Opera中可用。它提供了对`ipfs://`地址的支持，自动加载来自IPFS网关的网站和文件，允许轻松的导入和共享文件到IPFS，以及其他功能。

IPFS伴侣需要与本机的IPFS节点协同工作，因此安装插件前，确认已[安装节点](/install/ipfs-desktop/)。

## 安装

最简单的方式是通过浏览器的扩展程序应用商店来安装：

| [Firefox](https://www.mozilla.org/firefox/new/) \| [Firefox for Android](https://play.google.com/store/apps/details?id=org.mozilla.firefox)          | [Chrome](https://www.google.com/chrome/) \| [Brave](https://brave.com/) \| [Opera](https://www.opera.com/) \| [Edge](https://www.microsoftedgeinsider.com/)                                    |
| ---------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [![Install From AMO](https://ipfs.io/ipfs/QmWNa64XjA78QvK3zG2593bSMizkDXXcubDHjnRDYUivqt)](https://addons.mozilla.org/firefox/addon/ipfs-companion/) | [![Install from Chrome Store](https://ipfs.io/ipfs/QmU4Qm5YEKy5yHmdAgU2fD7PjZLgrYTUUbxTydqG2QK3TT)](https://chrome.google.com/webstore/detail/ipfs-companion/nibjojkomfdiaoajekhjakgkdhaomnch) |

确保本机已经安装了[IPFS](https://ipfs.io/#install)。因为IPFS伴侣（标准配置中）通过与本机IPFS节点的通讯来发挥作用，因此本机的IPFS节点也要处于运行状态。

## 特性

IPFS伴侣增强了浏览器的DWeb特性，提供如下功能：

### 检测IPFS路径格式的URL

IPFS伴侣检测并验证任何网站中IPFS-like的路径请求，如`/ipfs/{cid}`或`/ipns/{peerid_or_host-with-dnslink}`。如果该路径是有效的IPFS地址，则会重定向到本地网关中来加载此数据。位于`localhost`的网关也会将其转换为一个子域名，以给各个网站提供该数据的唯一数据源：

> `https://ipfs.io/ipfs/QmbWqxBEKC3P8tqsKc98xmWNzrzDtRLMiMPL8wBuTGsMnR`  
> → `http://localhost:8080/ipfs/QmbWqxBEKC3P8tqsKc98xmWNzrzDtRLMiMPL8wBuTGsMnR`
> → `http://bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi.ipfs.localhost:8080`

### 检测DNSLink-enabled URLs

IPFS伴侣在网站的DNS记录中检测DNSLink信息，如果一个网站中使用了DNSLink，IPFS伴侣将重定向该HTTP请求到本地网关中处理：

> `http://docs.ipfs.io`  
> → `http://localhost:8080/ipns/docs.ipfs.io` → `http://docs.ipfs.io.ipns.localhost:8080/`

### 检测包含`x-ipfs-path`头信息的页面

IPFS伴侣在任何HTTP响应中检测到`x-ipfs-path`头信息时，也会将交互传输升级为IPFS。这是一个备选方案，用于在请求的URL中没有包含IPFS路径时，仍然能够正确处理IPFS数据。

### 全局/单站点切换重定向

以下方式均可以禁用和重新启用本地网关重定向：

- 在IPFS伴侣的首选项中暂停全局重定向。
- IPFS伴侣的首选项的当前页切换按钮，可以暂停对应网站的重定向。
- 在URL的hash部分或者query参数中添加`x-ipfs-companion-no-redirect`描述。
example：[https://ipfs.io/ipfs/QmbWqxBEKC3P8tqsKc98xmWNzrzDtRLMiMPL8wBuTGsMnR#x-ipfs-companion-no-redirect](https://ipfs.io/ipfs/QmbWqxBEKC3P8tqsKc98xmWNzrzDtRLMiMPL8wBuTGsMnR#x-ipfs-companion-no-redirect)
example：[example：https://ipfs.io/ipfs/QmbWqxBEKC3P8tqsKc98xmWNzrzDtRLMiMPL8wBuTGsMnR?x-ipfs-companion-no-redirect](https://ipfs.io/ipfs/QmbWqxBEKC3P8tqsKc98xmWNzrzDtRLMiMPL8wBuTGsMnR?x-ipfs-companion-no-redirect)

### 在浏览器工具栏中访问常用的IPFS操作

IPFS伴侣使你在浏览器工具栏中简单点击，即可快速便捷的访问IPFS的常用操作：

- 直接在立方体图标上显示当前已连接的节点数。
- 单击图标显示主菜单，以查看IPFS API和网关状态。
- 在页面图片和其他元素上右键，即可轻松将其添加到IPFS中，并保留其文件名（如图片）。
- 选择主菜单中的导入功能，开启一个浏览器页面，该页面支持拖拽导入文件。
- 从主菜单中直接固定或者取消固定IPFS资源。
- 直接从主菜单中复制可共享的公共网关链接、IPFS内容路径或者IPFS资源的CID。
- 在主菜单中单击打开[IPFS网页仪表盘](https://github.com/ipfs-shipyard/ipfs-webui)。
- 从主菜单中快速切换网关重定向或者打开/关闭所有的特性。

## 更多文档

如果你想深入研究IPFS伴侣，查看该项目的文档[github.com/ipfs-shipyard/ipfs-companion →](https://github.com/ipfs-shipyard/ipfs-companion)
